---
name: 'D2: The Pipeline Moves There'
title: 'The collection and the pipeline move to that machine, so storage growth and model calls stop being two problems'
status: accepted
supersedes: []
blocked-by: [x4b6t, uta8s]
commits: []
references: []
---

## Question

The plan's milestones are agreed in order, each saying what it changes and what it cannot start before -- web's `spec/architecture/collection.md` holds the model they assume.

## Decision

- **The collection and the pipeline move there**: storage growth and the locality of model calls stop
  being two problems.
- **`local` is renamed in D2**, when it stops being local.
