---
name: The Enumeration Drifted
title: The enumeration is normative and the code has drifted from it
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

**Since answered: `check-css-enumeration` holds every StyleX declaration to the layer the
enumeration gives its property**, read from `apps/site/scripts/css-owners.ts`. What follows is the
finding as it was.

web's `spec/architecture/css/layers.md`, "The enumeration is the rule, and the test
is only how the enumeration grows", makes the property list the thing that decides, on the argument
that a lookup can be checked by somebody who was not in the argument. The list is prose inside
paragraphs, so nothing reads it, and nobody has checked it.

Measured 2026-09-21 over `apps/site/src`, taking every `stylex.create` block and every `class`
attribute in a `.svelte` file: 20 frame-owned declarations are written in the visual layer -- 11
`textWrap`, 6 `overflowWrap`, 3 `whiteSpace` -- and 14 vocabulary-owned ones are written as
utilities in the markup, 7 appearance and 7 color. The three `whiteSpace` are the sites that file
already accounts for as a different value at each; the other 31 are drift nobody recorded.

A table that is data rather than prose could be compared against what the tree declares. What it
costs is that the enumeration stops being readable where it is argued: why `cursor` splits by site,
why `will-change` belongs to no ramp, and the pair of mirror-image errors that placed the typography
line all sit beside entries a machine would rather have alone.
