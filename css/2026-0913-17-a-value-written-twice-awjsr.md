---
name: A Value Written Twice
title: 'A value is written twice on one element, once as a class and once in the visual layer'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `libs/prose/src/body.svelte`'s note close carries `focus-ring` in its
markup and `borderRadius: radius.sm` in its style object. `:where(.focus-ring, .focus-ring-inner,
.focus-ring-within)` in web's `apps/site/src/styles/utilities.css` already sets
`border-radius: 0.25rem`, and `radius.sm` is `0.25rem`, so the element is told the same thing twice
by two layers.

Nothing renders differently and nothing will until one of the two is changed alone, which is the
whole of the finding. That rule is inside `@layer components` and StyleX outranks it, so here the
visual layer's copy is the one that wins -- the opposite holds for the unlayered half of the same
file, which the entry below on the extraction threshold measures. It wins with the identical value,
so the redundancy is invisible from the browser and from every gate this repository has. A
component carrying a vocabulary class and restating one of its declarations has no way to say which
of the two it meant, and neither layer knows the other wrote it.

**One instance, verified by reading the markup and the style object together. No sweep was run**,
and the sweep is the part worth doing: the same shape is available anywhere a component carries
`focus-ring` or `focus-link` and writes a radius, a color or a transition of its own. What the
count would be is not known here. `quiet-control` was the third name in that list and is a recipe
now, where the same shape is not a redundancy but a merge: a component restating one of the
recipe's properties replaces its whole value for that property, which is a thing to get right
rather than to remove.
