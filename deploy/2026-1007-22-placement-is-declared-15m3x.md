---
name: Placement Is Declared
title: 'An app declares where it may run, a group and a place, never which nodes, and host deploys it where it matches'
status: declined
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Decided on 2026-10-08, so a node can join without an edit to every app that runs everywhere.

## Decision

- **An app declares where it may run, never which nodes**: a group -- `core`, `home`, `datacenter`, or
  `any` -- and, where it matters, a place by country or region, and host deploys it on every node that
  matches. `ledger` is the United States and `home`, which is `rdu` today and any machine at home in
  the US after it.
- **The core is the nodes with the most room and the best path to Cloudflare**, not the ones that hold
  Postgres: `rdu`, `tyo` and `buf`. `sha`, with 15 GiB, was measured and passed over: its tunnel lands
  in Los Angeles, connecting to Cloudflare in about 270 ms against `buf`'s 28 ms, and it downloaded
  from Cloudflare at 84 KB/s against `buf`'s 52 MB/s.
- **The database names its members apart from the core**, as it does today.
