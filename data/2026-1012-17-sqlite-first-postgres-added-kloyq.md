---
name: "SQLite First, Postgres Added"
title: "An app's database is an embedded SQLite with one writer, kept durable off its node, and Postgres is a capability added when it declares one"
status: accepted
supersedes: []
blocked-by: []
commits: []
references:
  - url: https://developers.cloudflare.com/d1/
    took: 'a database per app that is SQLite, written in one place and made durable beneath it, which most apps never outgrow'
  - url: https://litestream.io/how-it-works/
    took: "SQLite's WAL shipped continuously to object storage, so a database on one disk is restored elsewhere from it"
---

## Question

Two of the platform's rules disagreed about what an app's database is. Platform's
`spec/architecture/services.md` embeds SQLite in every service. `spec/architecture/scheduling.md`
gives each scope a Postgres holding the database of every app the organization runs. The author
asked on 2026-10-12 which one is the default, given that compute keeps nothing.

## Evidence

- The platform's own services all embed SQLite today, and none has needed more.
- One SQLite writer in WAL mode carries the writes of tens of thousands of users when its writes are
  queued and made one at a time. The writes that cannot be made that way are several apps, or
  several machines, writing one dataset at once.
- The platform itself is that case: every node and every service writes the registry, the ledger
  and the leases. It keeps them in Postgres, and in a cluster of its own -- platform's
  `spec/architecture/databases.md`.

## Decision

- **An app's database is SQLite by default, embedded in the app, in its own data directory, in WAL
  mode**, as Cloudflare's D1 gives a Worker. It is enough for most apps, and an app that does not
  need more does not pay for more.
- **One instance writes it**, placed on one node. An app may run more instances. Only the writer
  writes, and the others send their writes to it in a queue, without keeping state of their own.
- **It is kept durable off its node**: its WAL is shipped continuously, in Litestream's way, to its
  scope's bucket -- or to a store until the bucket exists, as the platform's backups already are. An
  instance moved to another node restores from there before it writes, so a database is pinned to
  its writer and never to its disk.
- **Postgres is a capability added when an app declares it** -- several writers on one dataset,
  several apps sharing one, or a feature only Postgres has. It is never the platform's cluster. Today
  it is the per-app sidecar of `databases.md`; whether a scope gets a cluster of its own, with
  replicas, is decided when an app first needs one.
- **A deploy's snapshot and a rollback with data stay whole**, since the SQLite is inside the app's
  subvolume as before; the shipped WAL is a copy for durability, not a second place to write.
