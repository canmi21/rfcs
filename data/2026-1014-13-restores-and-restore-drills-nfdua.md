---
name: 'Restores and Restore Drills'
title: 'A backup is restored, and proved by restoring it, only by hand with WAL-G, and whether a restore and a scheduled drill become workflows is open'
status: draft
supersedes: []
blocked-by: [23wgz]
commits: []
references: []
---

## Question

"A backup is proved by restoring it, not by its upload succeeding" -- platform's
`spec/architecture/databases.md`, "Backups are the data, kept off the cluster". The rule has no
schedule and no tool. Both restores so far were drills run by hand with WAL-G: on 2026-10-07, from B2
and from the mirror's sidecar before the mirror was deployed, and on 2026-10-08, when `rdu`'s
mirror, read as plain files, restored the cluster to the end of its archive on timeline 5 with every
database there. The 2026-10-07 drill found what an upload never shows: a B2 key with file access
alone answered `403` for a missing object, and on the first failover nothing was archived until it
was replaced.

A restore in anger is the same act under pressure: a table dropped, restored to the second before
within the last seven days, to a coarser moment beyond. It is row DA7 of platform's
`spec/architecture/operations.md`, with no RFC; RFC np65b asks who sees a backup that stopped, the
other half of trusting one.

What a restore takes is known: WAL-G, the major's image that took the backup, the store's
credentials and the key that decrypts it -- tier 0 under RFC 231yq -- an empty data directory, and a
target time. RFC 23wgz's workflows are the platform's way of running many-step work, and its first,
`mirror`, already runs after each base backup.

The ways in sight, and what each costs:

- **A drill as a scheduled workflow**: on a node with room, it restores the newest base backup and
  the WAL after it into a throwaway container, from B2 and from each mirror in turn, checks every
  database answers and a known row is there, records the result in the ledger, and removes it all.
  Against it: disk and time on the node it runs on -- a whole restore of the cluster each time -- and,
  unless it runs on a member, a node holding the key that decrypts backups, which today no node but
  the cluster's does.
- **A restore as a guarded action**, cpc2b's action asking a confirmation, naming the database and the
  moment: into a new database beside the old one, never over it, so taking it is a second, separate
  act. Against it: an action that writes a database as large as the live one, and a cut-over that is
  its own question.
- **A drill only, the restore kept by hand from a written procedure**: what the drill proves is the
  procedure, and a restore in anger is rare enough to be a person's. Against it: the act most needed
  under pressure stays the one with no tool.
- **How often**: monthly, weekly, after each major, or after each change to the backup's
  configuration -- the key change of 2026-10-07 being the kind a drill exists to catch.

What would decide between them: how much disk and time a drill may take and on which node; whether
the key that decrypts backups may live anywhere but the cluster's members; whether a restore in
anger is ever into the live database or always beside it; and how often a drill must run for a
failure it finds to still be cheap.
