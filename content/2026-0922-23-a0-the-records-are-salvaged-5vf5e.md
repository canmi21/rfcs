---
name: 'A0: The Records Are Salvaged'
title: 'One trustworthy media.yaml and tags.yaml out of the two divergent copies, and the path bug closed'
status: landed
supersedes: []
blocked-by: []
commits: [web@345bf335, web@0793e153, web@69cd2689]
references: []
---

## Question

Two copies of the curated records had diverged: the reader looked in the old place for records that
had moved five days earlier, and a second copy was being written outside git. A0 of web's
milestones.

## Decision

- **Every curated record is read from `data/record`**, the path bug closed.
- **The two copies of `media.yaml` and `tags.yaml` merged into the committed one**, and the tag lists
  the merge dropped restored.
- **A salvage, not a repair**: the file it fixes retires at A2, RFC vfusa.
