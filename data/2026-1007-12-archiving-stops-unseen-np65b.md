---
name: Archiving Stops Unseen
title: "A database that is up and not backed up is seen only in the ledger's failed run, and who sees it unasked is open"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`/health` now says when archiving has stopped, and the backup job fails on it -- platform's
`spec/architecture/databases.md`, "The container is Postgres and a keeper of it". What is still open
is anybody seeing it unasked: the probe cannot reach a socket on a node, and the console shows a
container's state, not what it answers, so a database that is up and not backed up is visible only in
the ledger's failed run, a day late at worst.

## Evidence

On `tyo`'s first start, on 2026-10-07, the image had no CA bundle: WAL-G could not verify the store's
certificate and retried without end, so both the base backup and the archiving of WAL hung, and
`/health` answered healthy throughout -- `pg_stat_archiver` counts a failure only when the archive
command returns, and this one never did. The bundle is in the image now, and `S3_MAX_RETRIES=3` makes
an unreachable store fail within seconds rather than an hour, so a failure is counted and logged.

A base backup can also hang rather than fail: once it has begun, Postgres waits at its end for the WAL
it needs to be archived, so archiving that stops during a backup holds the job until `cron`'s timeout.
Seen on `buf` on 2026-10-07; the five minutes before the job refuses to start do not cover it.
