---
name: 'The Census Skips :not()'
title: 'The scoped census does not descend into `:not()`'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`census.mjs`, the survey's scan, records `:not` as a reason a rule is relational without reading
its argument, so `footnotes.svelte`'s `.notes-fold:not([data-phase='expanded'])` counted as
ordinary despite the attribute selector inside it. It changed nothing this time -- that rule
carries `mask-image`, which the enumeration cannot answer for, so it stayed either way -- but the
next `:not()` wrapping something the scan cannot express will be mis-sorted the same way.
