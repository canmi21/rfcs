---
name: 'One Crate For Local State'
title: "The ground's local state is SQLite opened through one crate, which owns the pragmas, the migrations and what a newer file means, and each app keeps its own file and its own data"
status: accepted
supersedes: []
blocked-by: []
commits: []
references:
  - url: https://www.sqlite.org/pragma.html#pragma_user_version
    took: "a file's schema version in its header, read and written without a table"
  - url: https://www.sqlite.org/backup.html
    took: 'a consistent copy of a live database, page by page, while it is written'
---

## Question

Six programs of ours keep state in SQLite on their node, and each opens its file its own way. The
pragmas differ for no reason a reader can find: the relay and the probe set `synchronous = NORMAL`
and a busy timeout, host, the meter and cron set neither. Schema changes are made three ways:
`CREATE TABLE IF NOT EXISTS` and nothing else (cron, the probe, the meter, the relay), a column
added once it is found missing (host), and `user_version` stepped forward (rdm). Only the relay
does anything with a file it cannot open. None backs up its file itself. Each test helper is
written again.

RFC zlifc says the ground uses what an upper layer is good at as **code, not a service**. The
author asked on 2026-10-14 for one SQLite abstraction for the ground's local state: a crate, one
file per app, no data shared. This settles what the crate does, what it leaves to each app, and
the rule for a file written by newer code than the code opening it.

## Decision

- **One crate, `local-state`, in infra's `libs/local-state`, never published.** Decided with the
  author on 2026-10-14: what monoflake shares with itself is kept in infra, the lowest of its
  repositories, rather than added to `repos/lib`, which publishes what anyone may use; its name
  says the business it serves, an app's state on its own node, not that it is SQLite. Infra's apps
  take it by path, platform's by a git dependency pinned to a revision of infra, moved by the
  repository's `update` like any other and patched to the checkout beside it while one is changed
  with the other. It depends on `rusqlite` with `bundled` alone, which it **re-exports**: every
  user takes `libsqlite3-sys` through it, so each lockfile holds one, the collision
  `platform/apps/observe/probe/Cargo.toml` works around today cannot come back, and moving SQLite
  is one change.
- **What every user gets, the core:**
  - **Opening a file by its path, made if it is not there with its directory**, and an in-memory
    one for tests that keep nothing.
  - **The same pragmas for every file, set on every open**: `journal_mode = WAL`,
    `synchronous = NORMAL`, `busy_timeout = 5000`, `foreign_keys = ON`. A file whose loss of its
    last second's writes costs more than a fsync per commit asks for `synchronous = FULL` by name,
    as the meter's `usage.db` and the cue apps do now. NORMAL is WAL's own recommendation: a crash
    of the process loses nothing, a power cut the last commits since a checkpoint.
  - **Migrations, numbered from 1, each SQL or a function over the transaction, each declared
    `expand` or `contract`**, as the ledger's are (`platform/apps/data/ledger/src/migrations.rs`).
    Each runs in its own transaction with its record. The file's version is `user_version`; a table
    `migrations (version, name, kind, applied_at)` beside it keeps what each was, which is what the
    rule below reads. An `expand` is held to adding: a test the crate gives refuses one that drops,
    renames, retypes or truncates, as the ledger's `an_expand_only_adds` does.
  - **A file from before the crate is adopted**: version 0 with tables in it. Migration 1 is the
    schema as it stands today with `IF NOT EXISTS`, so it is a no-op on an existing file and a
    creation on a new one; a column an old file may lack is a later migration that adds it if it is
    missing, as host's `staged`, `cut` and `scoped` do now.
  - **One writer per file**: a connection behind a mutex that a poisoned lock does not wedge, and a
    `blocking` call that runs work on it off the async threads, as the relay's `runs::blocking`
    does. Readers that need a second connection, as the probe's do, open one read-only from the
    same handle.
  - **Test helpers**: a file in a temporary directory, reopened as after a restart, and a file at
    any earlier version, made by running the migrations up to it, so a test can open a file from
    before a change.
- **The rule for a file newer than the code: refused, unless everything newer is an `expand`.**
  An older build opening a file whose newer migrations are all `expand` opens it as it is and
  migrates nothing; one with a `contract` among them is refused, naming the file, its version and
  the code's. This is what host needs: keeper puts back the previous host when a new one does not
  start (infra's `spec/architecture/host.md`, "host rolls forward, not back"), and that previous host
  opens the four files the new one may have migrated. Today it works only because every change host
  ever made added a column, and nothing says so; the rule makes that the condition rather than the
  luck. A user's code is held to it in turn: it names its columns in every `INSERT` and `SELECT`,
  so a column it does not know is harmless. Refusing every newer file, as rdm does, would leave a
  host whose rollback cannot start, so the plain refusal is not the rule; it stays rdm's own
  choice, since rdm is not ground.
- **What a user chooses when its file cannot be opened, or is refused, by policy on open:**
  - **`fail`, the default**: the error is returned with the file's path, and the program does not
    start. Host, the meter's `usage.db`, cron and the probe's archive are the truth for what they
    keep, and a silent fresh file would erase it.
  - **`aside`**: the file and its `-wal` and `-shm` are renamed `<name>.broken-<unix seconds>`, why
    is logged, and a fresh file is made. For what is rebuilt from elsewhere: the relay's `runs.db`,
    rebuilt from host and its neighbors, which does exactly this today in
    `platform/apps/system/relay/src/runs.rs`. In the crate, since it is twenty lines that must be
    right about the WAL's files and are easy to get wrong.
  - **No integrity check on every open.** `quick_check` reads the whole file, and the relay's and
    the probe's grow for weeks; a check is a call a user makes when it wants one.
- **What only some users need stays out of the core:**
  - **Tiered roll-ups are not in the crate.** The two users fold differently: the relay folds each
    tier into the next in SQL as rows are dropped (`history/store.rs`, `fold.rs`, `outcomes.rs`),
    the meter folds in memory and writes each grain as it closes (`retention.rs`, `sampler.rs`).
    What they share is three lines of keeping a cut-off; a module for two users folding two ways
    would be a third way. Revisited when a third user folds as one of these does.
  - **Backup by the online backup API is a module of the crate behind a feature, `backup`, built
    when it has a user.** None has one now: the ground's files are snapshotted with their app's
    subvolume at each deploy (infra's host.md), and RFC zlifc puts backing up anything but the
    database above the ground. The first user is likely the ground sending its state up, which is
    "data up, never a dependency up".
- **What the crate never does**: replication, shipping the WAL anywhere, a network listener, a
  file shared between apps, or anything RFC 6jw04's overlay databases and RFC kloyq's shipped WAL
  are for. An app above the ground that wants a database is given one by the platform; this crate
  is for the state a node keeps of itself.
- **The TypeScript users are not in this crate**: the deployer, cue's relay and hub (`node:sqlite`)
  and web's collection (`better-sqlite3`). They are above the ground. If they are to share the
  rules, it is a sibling package beside the crate in infra's `libs`, holding the same pragmas and
  the same newer-file rule, decided on its own.
- **Moved in, simplest first**, each one change in its own repository, its tests unchanged in what
  they assert:
  1. **cron**, `platform/apps/system/cron/src/store.rs`: one table, no migrations; its open is
     fifteen lines. Gains NORMAL, the busy timeout and `fail`.
  2. **The probe's archive**, `platform/apps/observe/probe/src/archive.rs`: one table and an index,
     pragmas already the core's; its second connection becomes the handle's read-only one. Drops its
     `sqlx` note's reason, keeping the note for `sqlx-postgres`.
  3. **The meter**, `infra/apps/observe/meter/src/store.rs` and `ledger.rs`: three files, two
     schemas, no migrations; `usage.db` asks for FULL. Its in-memory grains stay its own.
  4. **The relay**, `platform/apps/system/relay/src/runs.rs` and `history/store.rs`: `aside`
     replaces its own move-aside, `blocking` its own; its four tiers' tables become migration 1
     beside `runs`; the folding stays.
  5. **host**, `infra/apps/deploy/host/src/store.rs`: four files; `staged`, `cut` and `scoped`
     become migrations 2 to 4 of their files, each `expand`; the reading of a legacy `host.db`
     stays host's own code. Last, since it is the one whose rollback the rule exists for, and a
     test opens each file at the version before, as keeper's put-back would.
  6. **rdm**, `rdm/src/store.rs`, if the author wants it: not ground, already on `user_version`
     (5), which the crate reads as its own version once its five steps are written as migrations.
  7. **The ledger's `import`**, `platform/apps/data/ledger/src/import.rs`, never: it opens an old
     file read-only once, and goes when the import is done.

### The survey

Lines are counted from the files as they are on 2026-10-14; "open" is opening the file and its
pragmas, "schema" is creating and changing tables, "tests" is the tests that open a database.

#### infra `apps/deploy/host`

- **What and why local**: four files in host's own data directory -- `apps.db` (what runs, what is
  held, the quota switch), `routes.db`, `history.db` (every event, never pruned) and `images.db`
  (images flagged for removal). Local because host is what deploys and repairs everything else
  (`spec/architecture/host.md`, "What host keeps, and where").
- **Open**: `open()` in `src/store.rs` sets `journal_mode = WAL` only; `synchronous`, the busy
  timeout and foreign keys are defaults. One connection per file, each in a `Mutex` whose poison is
  ignored, called from async handlers directly. About 15 lines.
- **Schema**: `CREATE TABLE IF NOT EXISTS` with today's full shape, then `staged`, `cut` and `scoped`
  each read `pragma_table_info` and add a column, with an update, if it is missing; no version is
  kept. A single `host.db` from before the split is copied into the four once and renamed
  `host.db.split` (`read_legacy`). About 120 lines for the schema and its changes, 50 for the legacy
  split.
- **Older code on a newer file**: works today, because every change added a column and host names
  its columns. This is the case keeper's put-back creates.
- **Unopenable or corrupt**: the error propagates, host does not start, and keeper puts back the
  previous host.
- **Retention**: none; every event is kept.
- **Backup**: none of its own.
- **Tests**: `src/store/tests.rs`, 243 lines, each on `tempfile::tempdir()` and `Store::open`.

#### infra `apps/observe/meter`

- **What and why local**: `hours.db` and `containers.db` (`src/store.rs`, one shape: minutes for a
  day, quarters of an hour for twelve, hours for good) and `usage.db` (`src/ledger.rs`: each hour's
  exact bytes, the counters they were read at, boots, gaps, disk and groups). Local because the
  meter shows a node to its operator while everything above is down (RFC zlifc names its history
  past its days as what goes up).
- **Open**: WAL on all three; `usage.db` adds `synchronous = FULL`, since its hours and counters
  must land together. One connection each, owned by the `Sampler` behind one `Mutex`, ticked from a
  thread of its own. About 20 lines for the hours, 65 for `usage.db`'s seven tables.
- **Schema**: `CREATE TABLE IF NOT EXISTS` only; never changed.
- **Unopenable or corrupt**: the error propagates; the meter does not start.
- **Retention**: seconds in memory for 15 minutes; minutes, quarters and hours folded in memory
  (`src/retention.rs`, 301 lines, and `src/sampler.rs`) and each written as it closes, the shorter
  grains `forget`-ten past their keeping; hours and usage kept for good.
- **Backup**: none of its own.
- **Tests**: `tempfile` files throughout; `src/store.rs` about 50 lines, `src/usage/tests.rs` 389,
  one of which opens the file raw to count rows.

#### platform `apps/observe/probe`

- **What and why local**: `probe.db`, every round of every check, for good (`src/archive.rs`), the
  archive the public `probe` scope answers from and the thinning to Supabase reads.
- **Open**: WAL, NORMAL, busy timeout 5000. Two connections: the writer owned by the writing task,
  the reader in an `Arc<Mutex>` for the API (`src/main.rs`). About 18 lines.
- **Schema**: one table and one index, `IF NOT EXISTS`; never changed.
- **Unopenable or corrupt**: the error propagates.
- **Retention**: none on the node; the archive grows. Thinning happens in Postgres.
- **Backup**: none.
- **Tests**: `tempfile`, about 50 lines in `src/archive.rs`.
- **Note**: `Cargo.toml` uses `sqlx-core` and `sqlx-postgres` rather than the `sqlx` facade
  because the facade's optional `sqlx-sqlite` pulls a second `libsqlite3-sys`.

#### platform `apps/system/relay`

- **What and why local**: `runs.db`, every node's host events for 30 days and at most 5,000 a node,
  mirrored on every relay (`src/runs.rs`), and in the same file the minutes and their folds
  (`src/history/store.rs`). Local because the relay is how the console sees the ground with the
  platform down.
- **Open**: WAL, NORMAL, busy timeout 5000. One connection in an `Arc<Mutex>`, every use through
  `blocking`, `spawn_blocking` with the lock. About 55 lines with the move-aside and 20 for sharing.
- **Schema**: `IF NOT EXISTS` for `runs` and its two indexes, and for `minutes`, `hours`, `days` and
  `run_days`; never changed.
- **Unopenable or corrupt**: if opening or creating the tables fails, the file and its `-wal` and
  `-shm` are renamed `runs.db.broken-<seconds>`, logged, and a fresh one is made: the mirror is
  rebuilt from host and the neighbors. A corruption found later, in use, is an error of that use.
- **Retention**: runs by start and count, deleted in the write's transaction. Minutes for two days,
  hours for 30, days for 400, each tier folded into the one above as it is dropped, and each day's
  runs counted into `run_days` before its first row goes (`history/store.rs` 337 lines, `fold.rs`
  219, `outcomes.rs` 313).
- **Backup**: none; it is a mirror.
- **Tests**: `Store::memory()`, `open_in_memory`, for the mirror; about 300 lines in `src/runs.rs`
  and 70 in `history/store.rs`.

#### platform `apps/system/cron`

- **What and why local**: `cron.db`, each job's last due time and last run, so a restart knows what
  it missed (`src/store.rs`).
- **Open**: WAL only. One connection in a `Mutex` in the scheduler. About 15 lines.
- **Schema**: one table, `IF NOT EXISTS`; never changed. A last run that no longer parses is passed
  over when read.
- **Unopenable or corrupt**: the error propagates.
- **Retention**: one row a job.
- **Backup**: none.
- **Tests**: `tempfile`, about 33 lines, reopened as after a restart.

#### platform `apps/data/ledger`

- **What**: no local state; its history is Postgres. `src/import.rs`, 296 lines, opens the SQLite
  it was kept in before, `SQLITE_OPEN_READ_ONLY`, reads every schema it ever had, and copies it
  once. Tests build files of the old schemas.

#### rdm (`rdm/src/store.rs`)

- **What**: the download list of a desktop app, `internal.sqlite`. Not ground.
- **Open**: WAL. One connection.
- **Schema**: `user_version`, 5, stepped forward one `ALTER` at a time; **a file newer than the
  build is refused** with its version and the build's. About 90 lines.
- **Tests**: `tempfile`, about 140 lines.

#### TypeScript, above the ground

- `platform/apps/system/deployer/src/store.ts` (`node:sqlite`, a column added if missing, as host).
- `cue/apps/relay/src/queue.ts` and `cue/apps/hub/src/store.ts` (`node:sqlite`, WAL, FULL, the hub
  a busy timeout).
- `web/libs/collection/src/open.ts` (`better-sqlite3` and Drizzle, WAL and `foreign_keys = ON`, the
  one user that turns foreign keys on).
- `web/apps/site/api` reaches D1, which is Cloudflare's, not a file.
