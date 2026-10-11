---
name: 'A7: One Rid Over Every Language'
title: "The existing views and translations move under the article's own identity"
status: accepted
supersedes: []
blocked-by: [huanh]
commits: []
references: []
---

## Question

The plan's milestones are agreed in order, each saying what it changes and what it cannot start before -- web's `spec/architecture/collection.md` holds the model they assume.

## Evidence

**A7 is surveyed and not taken.** The seven sidecars hold 2,912 translations over 364 segments
and eight locales, and every one of them keys by the hash of the sentence rather than by the
article -- which is why none is shared between two articles today and why that is still the right
key: the money was spent per segment, and the same sentence written twice should be paid for
once. Nothing collides with what the collection already holds, and no field in them has nowhere
to go. So the import is a decision about the owner and not a migration with a risk: `segment`
rather than a rid, with "one rid over every language" describing the article above them, which
already has one.

## Decision

- **The existing views and translations move under the article's own identity**, the segment, not
  a rid, owning each translation.
