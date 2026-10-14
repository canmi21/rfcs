---
name: 'Shared Code Has One Home'
title: 'What two of our programs both do is written once, in a crate named for the role it serves, in the lowest repository every user can read, and the facts the console shows are read, not copied'
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

On 2026-10-14 the author noticed that six programs open SQLite six ways, settled by RFC 30mks, and
bet that the same was true across the rest of the code. Two surveys read every Rust crate in infra
and platform and every TypeScript program and task in web, platform, infra and lib. They found
twenty-one Rust findings and eighteen TypeScript ones, and in eleven of them one copy had fixed what
another had not: keeper admitted every request with `HOST_TOKEN` set and empty, which host refuses;
secrets gave a node every app the registry listed for it, without the placement check host makes;
database wrote `patroni.yml`, passwords and all, world-readable before it set the mode; platform's
image task lacked infra's pinned compiler and reproducible timestamps; the console's API answered a
thrown error as plain text, which site's API was written to prevent; and others. About 2,050 lines
of tasks are kept identical between platform and infra by hand, and nothing checks that they are.
The console copies the app list, 37 display names, the nodes' coordinates and the cores by hand;
the executor's arrival broke web's check that day for want of one more line.

## Decision

Decided with the author on 2026-10-14. RFC 30mks's rule is the frame: what monoflake shares with
itself lives in infra's `libs/`, unpublished, taken by path in infra and by a git dependency pinned
to a revision in platform; `repos/lib` keeps only what anyone could use; a crate is named for the
role it serves, never its technology.

- **The bugs are fixed first, in place**, each in the copy that had it, with a test, before any
  extraction moves the code: a fix is not waited on behind a refactor.
- **Five crates, named and placed now:**
  - `hosted-app`, in infra's `libs/`: what host expects of an app it runs -- SIGTERM and serve,
    `LISTEN` and the app's data directory, reading its environment, atomic and private writes, the
    Unix-socket door, the tests of its `service.toml`. Sixteen copies of shutdown and thirteen of
    file writes today. First.
  - `reach`, in infra's `libs/`: one of our programs asking another and admitting it -- the HTTP
    client with its TLS provider named, a timeout over the body, a cap on it, the envelope decoded,
    the first core that answers, bearer admission, and fakes for tests. Fourteen call sites over
    four client stacks and seven token compares today. With `hosted-app`.
  - `wanted`, in infra's `libs/`: what the registry wants, as the ground reads it and keeps it
    between answers, with the placement check. Three copies.
  - `node-contracts`, in infra's `libs/`: the files host writes for the apps on its node, each one
    type both the writer and the reader use, and each carrying its `version` as `spec/json.md` says.
  - `control-state`, in platform's `libs/`: what a control service needs of Postgres --
    migrations, the pool, test schemas -- the counterpart of `local-state`. Three copies; built
    before RFC 6jw04's overlay databases would make a fourth.
- **`response`, in `repos/lib`, gains what any user of the envelope needs**: decoding an answer
  into a result, a one-line catalog test, and loading a service's own codes, RFC v7g2p. It is
  general, so it stays published.
- **A task two repositories run is written once, in the workspace's `.mise/tasks`.** CI already
  checks out the workspace, and `refs_check.py` is the precedent. That takes `artifact.ts`,
  `deploy-base`, `fingerprint.py`, `resolved.py`, `graph.py` and their tests out of platform and
  infra, and `secrets`, `comments`, `clean`, `outdated` and the inline `fmt`, `update`, `lint` and
  `test` out of web, platform and infra. A repository keeps a task of its own only for what is its
  own. What moves from Python is ported to TypeScript as it moves, the workspace's rule for a new
  task.
- **The console reads the facts it shows from where they are kept.** The app list, each app's
  display name, the nodes and their coordinates, and the cores come from the registry, which
  already imports the nodes and the apps and gains the rest; the console's hand-kept copies and the
  tests that hold them against other repositories go. A fact the console needs before the registry
  answers is the registry's last answer, as host keeps it.
- **The order**: the bugs; then `hosted-app` and `reach`; the workspace tasks; `response`'s
  additions; the console reading the registry; `wanted` and `node-contracts`; `control-state`
  before the first overlay database. Each extraction moves one user first and the rest after it
  passes.
- **Not extracted**: the doubling backoffs, the rolling tiers of the meter, the relay and the probe,
  mutex poison helpers, and each task's `fail()` and `run()` -- each is a few lines whose users
  differ more than they agree.
