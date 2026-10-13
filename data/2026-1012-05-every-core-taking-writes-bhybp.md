---
name: "Every Core Taking Writes"
title: "One core writes the platform's Postgres and two may take over, and whether every core should take writes is open"
status: draft
supersedes: []
blocked-by: []
commits: []
references:
  - url: https://www.postgresql.org/docs/current/logical-replication-subscription.html
    took: "`origin = none`, which lets two servers subscribe to each other without sending a change back, so writes can go both ways; conflicts are logged, not prevented"
  - url: https://github.com/aws/pgactive
    took: 'an active-active extension under Apache 2.0, resolving a conflict by the last update'
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
