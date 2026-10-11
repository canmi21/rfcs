---
name: 'C1: The Collection Moves Out'
title: 'The collection lives on its own, and local is the only process that may write it'
status: accepted
supersedes: []
blocked-by: [ex27b]
commits: []
references: []
---

## Question

The plan's milestones are agreed in order, each saying what it changes and what it cannot start before -- web's `spec/architecture/collection.md` holds the model they assume.

## Decision

- **The collection lives on its own, and `local` is the only process that may write it.** What is left
  for it is growth and D2's move to another machine, neither of which presses today.
