---
name: 'Every Core Taking Writes'
title: 'One core writes; what must not be disputed is confirmed by two of the three cores, the rest written and read near'
status: accepted
supersedes: []
blocked-by: []
commits: []
references:
  - url: https://www.postgresql.org/docs/current/logical-replication-subscription.html
    took: '`origin = none`, which lets two servers subscribe to each other without sending a change back, so writes can go both ways; conflicts are logged, not prevented'
  - url: https://github.com/aws/pgactive
    took: 'an active-active extension under Apache 2.0, resolving a conflict by the last update'
  - url: https://www.postgresql.org/docs/current/runtime-config-replication.html#GUC-SYNCHRONOUS-STANDBY-NAMES
    took: '`ANY 1 (...)`, a commit waiting for any one of the standbys named, and `synchronous_commit` set per transaction'
  - url: https://patroni.readthedocs.io/en/latest/replication_modes.html
    took: 'quorum commit mode, which promotes only a member holding every commit a client was told succeeded'
---

## Question

The platform's Postgres has one writer, `tyo`, and `sin` and `rdu` may take over in that order --
platform's `spec/architecture/databases.md`, "Where it runs, and which one writes". A write from the
US crosses the Pacific to reach it. The author asked on 2026-10-12 whether the three cores could
each take writes, replicating to each other behind, so that neither a write nor a read crosses a
continent.

What a write needs decides whether it can be merged after the fact:

- **Appended rows on natural keys** -- the ledger's tasks and events, a node's minutes -- merge
  without conflict; the ledger already decides which write wins in SQL.
- **A name that must be unique** -- a bucket's names and references, a scope, an account -- cannot:
  two regions taking one name both answer success, and the merge drops one after its writer moved on.
- **A lease** -- the deployer's, the platform scheduler's -- cannot at all: two regions each hold it,
  and a job runs twice, a run is deployed twice.
- **What a person sets in the console** merges, and is written seldom enough that the Pacific is not
  felt.

Logical replication also carries no schema change, so every migration runs on each writer, and the
ledger's migration lock, an advisory lock, holds on one server only; sequences cannot be used; and
Patroni does not manage such a cluster. What it would give: it replicates between architectures, so
an x86 node could run Postgres natively rather than emulating arm64.

## Options

- **Keep one writer, and stop crossing a continent where it is not needed**: reads from a standby on
  the reader's own node or region, through a read path beside `primary`; appended rows written
  locally and sent on to the primary behind, idempotent as they already are; leases, unique names
  and settings written to the primary, across the ocean, at the rate they are written.
- **Every core takes writes, each table owned by one region**, so no two regions write one row:
  leases and unique names kept out of Postgres, in etcd or the relay's log.
- **Every core takes writes, conflicts resolved by the last update**, which the leases and names
  above cannot survive.

## Evidence

`sin` became a core on 2026-10-12, 73 ms from `tyo`. A commit that waits for one other core
waits for that, not for the 150 ms to the US that made replication asynchronous. Replication is
asynchronous today, and a failover can lose up to 1 MiB of WAL that a client was told was written --
platform's `spec/architecture/databases.md`.

## Decision

Decided with the author on 2026-10-12: **one core writes, and each write is classed by what losing
it costs**, not by which core is nearest.

- **What must not be disputed is confirmed by two of the three cores before it is answered**: what
  is wanted (a resource's `spec` and its revision), a lease, a unique name, and the database's and
  the quorum's membership. The primary waits for `ANY 1` of the other two cores, under Patroni's
  quorum mode, so a failover promotes only a core holding every such commit. Inside Asia this costs
  one round trip to `sin`; `rdu` answers when `sin` cannot. What remains at risk is a write
  confirmed by `sin` alone when Asia is lost whole.
- **What can be replayed is written near, by one node**: a ledger event, a status a controller
  reports, a measurement. Such a write sets `synchronous_commit = local`, and in time it goes to the
  node's own store first and is sent on to the primary behind, idempotent on its natural key. A
  status lost is reported again on the next round, and an event sent twice is the same row.
- **Reads are near, from the reader's own standby.** Correctness is held at write time: a write
  carries the revision it read, RFC cpc2b, so a stale read is refused when it is written rather than
  trusted. Two exceptions. A lease is taken on the primary only. A writer that reads back what it
  wrote waits until its standby has reached the revision its write answered, or reads that one
  answer from the primary.
- **The platform is scheduled from the primary's node.** Its scheduler runs on each core, and only
  the copy on the node that leads the database takes the lease. It takes it by a confirmed write
  that carries a fencing number, which grows with every new holder, so a holder that comes back late
  is refused. The scheduler follows the primary that etcd and Patroni already chose, and adds no
  second election.
- **The cores do not each take writes.** Leases and unique names cannot be merged after the fact,
  and what every core taking writes would buy is had by writing replayable work near.
