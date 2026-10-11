---
name: 'C2: Development Gets Copies'
title: 'Development reads published objects from the live CDN and dumps reader state once, never a full copy'
status: accepted
supersedes: []
blocked-by: [x4b6t]
commits: []
references: []
---

## Question

The plan's milestones are agreed in order, each saying what it changes and what it cannot start before -- web's `spec/architecture/collection.md` holds the model they assume.

## Evidence

C2 is why the collection growing to a terabyte is not a problem that has to be solved later.
Published objects are immutable and already served, so development reads them where they are; the
only thing that must be copied is the reader state, which is a few megabytes of relational data.
Nothing about this arrangement changes as the photography lands.

## Decision

- **Published objects read from the live CDN, reader state dumped once; no full copy, ever.**
