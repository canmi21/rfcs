---
name: 'Cron and the Ground'
title: "cron starts the database's daily base backup, which the ground must keep, yet links the ledger's client and is not on the ground's list, and whether it is ground is open"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

RFC zlifc keeps the ground to what repairing the platform needs, by one test: with every service
above the store down, is it needed to deploy, reach, see or restore the platform? Its list is host,
keeper, Caddy, the tunnel, the resolver, the relay and the meter. It names "backing up anything but
the database" as work built above, so backing up the database is not. RFC l6r4u, grouping the
platform's apps on 2026-10-14, put `cron` in `node` beside the relay and the package agents, as
"what every node is granted by default". It left open whether that puts `cron` in the ground.

`cron` is how the database is backed up. The database's `service.toml` declares two jobs: `backup`
at 10:00 UTC, the daily base backup, and `amcheck` at 14:00. `cron` on each node asks them through
Caddy on the private side when they fall due -- platform's `spec/architecture/cron.md`. The WAL is
archived by Postgres itself, so a missed base backup loses no write. It does lengthen a restore and
break the tiers' daily rhythm. `cron` also runs `apt`'s and `apk`'s weekly upgrades, spread a day
apart across the nodes, and every other app's jobs, `geo`'s refresh among them.

`cron` also links the ledger's client (`apps/system/cron/Cargo.toml`), and every run is a ledger
task of its own. The client waits on nothing: "a ledger that is down costs records, never a capture"
-- `ledger.md`, and `cron`'s own `api.rs` builds a ledger nobody answers for its tests. That may
already be zlifc's "data up, never a dependency up", but the check zlifc describes would read it as a
reference from a lower layer to a higher one. And `cron` is platform code, while zlifc counts
infra's six apps as the ground.

The ways in sight, and what each costs:

- **`cron` is ground**, added to the list by this RFC, as zlifc asks of any addition, with its
  ledger client classed as data sent up. It is code the ground already runs on every node. Against
  it: the ground grows by a scheduler, its list and its repository disagree, and the layer check
  needs a rule for a client that waits on nothing.
- **`cron` stays above, and the backup is started from below it**: the database's keeper runs its
  own daily base backup on a timer, as it already runs WAL-G, and `cron` remains for
  jobs that can wait. Against it: a second scheduler for one job, and the console's Schedules page,
  RFC 68xsm, would no longer see the database's most important job.
- **host runs the schedule itself**: host already writes every app's schedules to
  `schedules.json`, so it could ask each job itself, and `cron` would retire. Against it: host grows,
  and every job's run lands in host's history rather than a ledger task.
- **The jobs are split by layer**: a job declares its layer with its app, a ground `cron` runs the
  ground's and the store's, and the rest go to the platform's scheduler, RFC z9vqb. Against it: two
  schedulers to explain, each with half the jobs.

What would decide between them: whether a base backup missed while the platform's services are down
fails zlifc's test, given the WAL is still archived; whether a client that drops what it cannot send
counts as a run-time reference up; whether the weekly package upgrades are repair; and whether a
ground app may live in the platform's repository or moves to infra if it is ground.
