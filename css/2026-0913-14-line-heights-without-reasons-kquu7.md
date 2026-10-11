---
name: Line Heights Without Reasons
title: 'A line height with no reason behind it is a lookup, not a name'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`1.4` is the site's third repeated line-height ratio -- web's `libs/prose/src/blocks/cargo/cargo.svelte`,
web's `libs/prose/src/blocks/tokei/tokei.svelte` and
web's `libs/prose/src/blocks/mermaid/mermaid.svelte` -- so it clears the three-component
threshold that every other name in the vocabulary was admitted by. It was left a literal anyway.

Nothing distinguishes it. It is on no scale: Tailwind's neighbors are `leading-snug` at 1.375 and
`leading-normal` at 1.5, and no utility writes 1.4. No comment beside any of the three says why.
Two of the three are the same block copied -- cargo's tooltip and tokei's are the same eight
declarations in the same order -- so the three components are closer to two decisions than to
three. A
name for it would have to say where it sits in a list of three, which stops being true the moment
there is a fourth, and a reader following the name arrives at the same three characters they
started from.

That is the same shape as the `0.71875rem` and `0.78125rem` the entry above names, one step
further on: there two components each rounded a judgment, here three landed on one number and
nobody chose it. Deciding it is either moving each of the three onto the scale, which is a visual
change on at least two of them and therefore needs the gate rather than an argument, or writing
down that a scale is allowed members nobody chose -- in which case the threshold for a name is not
repetition alone, and web's `spec/architecture/css/extraction.md` says it is.
