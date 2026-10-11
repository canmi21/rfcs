---
name: Ledger in the Cluster
title: "ledger's SQLite becomes its database in the cluster, the first app given one and the proof of database grant"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The platform is to run services that keep nothing on their own nodes, in order, each deployed and
proved before the next -- platform's `spec/architecture/scheduling.md`. What comes first?

## Decision

- **`ledger` moves into the cluster**, in `platform`: its SQLite on `rdu` becomes its database there,
  and it runs on all three cores.
- **It is the first app given a database**, and so the proof of `database grant`.
