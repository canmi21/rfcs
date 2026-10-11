---
name: 'C0: The Unrecomputable Backed Up'
title: 'A real backup of the authored database, the id register and the paid outputs, before the corpus leaves git'
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The plan's milestones are agreed in order, each saying what it changes and what it cannot start before -- web's `spec/architecture/collection.md` holds the model they assume.

## Evidence

**C0 waits on nothing but the database existing.** It was ordered after C1 while C1 was assumed to
come first; a backup does not need the collection to have moved, and the things it protects are
already written. C1 is the one deferred, so C0 stops waiting on it.

**C0 is ordered before C3 and the order is the point.** Git is what currently backs up the things
no run can reproduce: the id register, whose entries are identities other records point at, and the
paid descriptions. Losing them is not a rebuild, it is a re-purchase or a permanent loss of
identity. So the backup exists before the corpus stops being committed, not after. A0 is the
evidence that this is not hypothetical -- five days of curation are already outside git today.

## Decision

- **A real backup of the authored database, the id register and the paid outputs.**
