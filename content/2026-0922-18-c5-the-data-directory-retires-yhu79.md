---
name: 'C5: The Data Directory Retires'
title: "Nothing under data/ is tracked, because nothing there is git's to keep"
status: accepted
supersedes: []
blocked-by: [w3i3n, vfusa]
commits: []
references: []
---

## Question

The plan's milestones are agreed in order, each saying what it changes and what it cannot start before -- web's `spec/architecture/collection.md` holds the model they assume.

## Evidence

**C5 is the end of this group and the measure of whether the rest worked.** `data/` was tracked in
30 files, measured on 2026-10-05, and each one is tracked for a reason that some milestone above
removes:

| what                                                                             | files | what has to happen first                                       |
| -------------------------------------------------------------------------------- | ----- | -------------------------------------------------------------- |
| `record/media.yaml`, `record/tags.yaml`, `record/metadata.json`                  | 3     | imported already; the readers move to the collection           |
| `record/diagram.json`, `fonts.json`, `indexnow.json`, `licenses.yaml`, `tn.yaml` | 5     | each is decided: a row in the collection, or a file that stays |
| `build/*.json`                                                                   | 4     | derived, but fetched over a network; they become derived rows  |
| `record/symlinks.json`                                                           | 1     | done for the bytes: they are objects, the record names them    |
| `source/favicon/*`                                                               | 15    | bytes, refetchable, and cheaper to back up than to refetch     |
| `bucket/*/.gitkeep`                                                              | 2     | the skeleton, which goes when the tree it marks does           |

The site's own marks were the row that decided the order: losing them is not a rebuild. They left
git before C0 by the author's decision, as objects named by a record -- the bucket and its mirror
hold the bytes, and C0 is still what makes that more than two copies. See
platform's `spec/architecture/delivery.md`, "Every fixed name is a record".

## Decision

- **Nothing under `data/` is tracked**, because nothing there is git's to keep.
