---
name: Over the Line Limit
title: 'Over the line limit, and deferred'
status: draft
supersedes: []
blocked-by: [vfusa, qsizp]
commits: []
references: []
---

## Question

The workspace's line check measures every file, prose and data included, and these are over its
hard limit of a thousand lines. Each is marked `lines=deferred` in `.gitattributes`, so the check
reports it and passes, and each is split after the migration into layers rather than during it.

- `data/record/media.yaml` and `tags.yaml`, the curated records, which A2 moves into the database
  -- RFC vfusa; they leave rather than split.
- six of the translation sidecars, `contents/**/*.i18n.yaml`, the longest at 9329 lines, whose shape waits
  on the segment layer's redesign -- see RFC qsizp.

A deferral whose file falls under the limit is reported too, so the mark does not outlive it.
