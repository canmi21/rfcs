---
name: 'Databases Made by the Platform'
title: "A database is made, placed and kept by the platform through its API, its engine is the code's to declare, and moving engines is a migration with a short stop"
status: accepted
supersedes: []
blocked-by: [kloyq, 231yq, 23wgz]
commits: []
references: []
---

## Question

An app's database is SQLite by default and Postgres when declared -- RFC kloyq -- but making one is
still an edit to git and a task run by hand. The author asked on 2026-10-13 whether a database can be
made by one call to the platform, as a bucket is to be, and whether an app outgrowing SQLite can be
moved to Postgres in place.

## Decision

Decided with the author on 2026-10-13.

- **A database is a resource of kind `database`**, in RFC cpc2b's shape: written from the console or
  the operator's task, never by an edit to git. A controller converges it. It makes the database and
  its login, keeps its credentials in the secret service (RFC 231yq), and binds them to the app, so an
  app is given a database by one write.
- **Its engine is the code's to declare.** An app's `service.toml` says SQLite or Postgres, because
  the SQL it sends is written for one of them. Where the database is, how many copies are kept, and
  how it is backed up are the platform's, and are changed by the API.
- **Moving from SQLite to Postgres is not done in place.** The app's code must change for it, so the
  platform cannot do it behind the app's back. SQLite has no logical replication to follow while the
  app writes, and a change log built for it would cost far more than it saves.
- **It is a migration the platform guides, with a short stop to writes**, run as a task of stateless
  steps (RFC 23wgz) the operator starts:
  1. The app ships a version that speaks Postgres.
  2. The platform makes the Postgres database.
  3. It copies the data across, translating the schema, and checks it by counts and checksums.
  4. Writes stop. SQLite has one writer, so this is that writer turned read-only.
  5. What changed meanwhile is copied.
  6. The binding moves to Postgres and the new version is deployed.
  7. The SQLite file is kept for some days, for going back.

  The stop lasts seconds to minutes, by the size of the data.

- **It is built when an app first outgrows SQLite**, not before: most never will.
