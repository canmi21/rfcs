---
name: Retention per App
title: 'Every bucket keeps what it deleted for seven days, and where an app declares a period of its own is open'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Every period a bucket has is seven days -- platform's `spec/architecture/scheduling.md`, "Deleting
releases a name, and the bytes go later": a deleted reference is restorable for seven days, and
unreferenced bytes are reclaimed seven days after. Apps will want their own: a user's trash kept for
a month, a cache's leftovers gone within the hour, and different parts of one app different again.

Where such a period is declared -- the app's `service.toml`, the bucket's layout, or a row the app
writes at run time -- and how finely, per app, per key prefix or per reference, is undecided.
