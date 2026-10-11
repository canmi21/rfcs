---
name: The Unnamed Ledger
title: 'The `// unnamed:` ledger has no entries and no counter'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

**Since answered in part: `check-css-ramp` counts the ramp values written as a literal**; whether
the marks themselves are kept is still open. What follows is the finding as it was.

web's `spec/architecture/css/authoring.md`, "An unnamed ramp value is marked, so
it can be counted", asks for a mark beside every ramp property written with a literal and a gate
that prints the total. Measured 2026-09-21: the tree carries zero marks, and no task counts
anything. The convention is written and unused, which is worse than either half alone -- a reader
meeting an unmarked literal cannot tell whether it was judged and passed or never looked at.

**The 125 that section names is not reproducible and should not be read as current.** Scanning the
same properties today finds 48 written with a literal across the site's own `.svelte` and `.ts`
sources against 145 taking a named value, plus 27 in stylesheets outside the visual layer;
`libs/fonts` is excluded, where the same property names are `@font-face` descriptors and not a ramp
at all. Which of those sets the gate should count is part of what deciding this costs, and the
number moves by a factor of two depending on the answer.
