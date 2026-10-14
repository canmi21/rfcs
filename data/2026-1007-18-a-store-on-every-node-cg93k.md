---
name: A Store on Every Node
title: 'Every node gets a store on its own disk, S3 over its filesystem'
status: accepted
supersedes: []
blocked-by: [u7prm]
commits: []
references: []
---

## Question

A scope's bucket is laid out across stores, and the nodes' own disks are to be among them -- platform's
`spec/architecture/scheduling.md`.

## Decision

- **A store on every node's disk, S3 over its filesystem**, fourth of the services that keep nothing.
- `rdu` and `buf` have theirs, holding the backups' daily mirror since 2026-10-08 -- platform's
  `spec/architecture/databases.md`, "Backups are the data, kept off the cluster"; the other nodes'
  follow when a bucket's layout first names them.
