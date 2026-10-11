---
name: Migrations Through the Platform
title: "An app's migrations run as it pleases, which replication cannot carry, and whether the platform runs them is open"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Every app's migrations today run against the primary over its own `DATABASE_URL`, as the app pleases.
Logical replication does not carry them: a column added on the primary stops a subscription that does
not have it, and a table created there is never copied. The cluster no longer needs a subscriber to
span architectures -- platform's `spec/architecture/databases.md`, "Where it runs, and which one
writes" -- but a major's rolling upgrade is one, for as long as it runs, and a friend's app the
platform must answer for would want its schema known.

A migration runner of the platform's would: an app hands it its migrations, and it applies each to the
primary and to every subscriber, in order, and records which have run. It would also be where an app's
schema version is known, which a major's upgrade and the one address on every node would read. What
an app hands it, how a migration that fails halfway is undone, and whether it is a service or a step
of deploying an app are undecided, and so is whether it is wanted before there are apps of anybody
else's: until then an app runs its own migrations, as `ledger` will, each change made in two steps so
the old version and the new survive it together.
