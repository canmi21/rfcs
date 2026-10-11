---
name: The Console Shows Schedules
title: "The console shows cron's jobs and the ledger's records, which infra's panel showed until it retired"
status: accepted
supersedes: []
blocked-by: [2p4a1]
commits: []
references: []
---

## Question

Infra's panel showed the schedules and the tasks until it retired, and nothing shows them since.

## Decision

- **The console shows the schedules and the tasks**: cron's jobs and the ledger's records. Its
  Schedules page lists every job -- its node, its service, its schedule, when it runs next, how its
  last run went -- and runs one now -- platform's `spec/architecture/cron.md`.
