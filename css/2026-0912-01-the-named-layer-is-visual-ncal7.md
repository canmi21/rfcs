---
name: The Named Layer Is Visual
title: 'The named layer in CSS is the visual layer, written before there was one'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`utilities.css` and `@canmi/ui/primitives/style.css` hold a
vocabulary of named surfaces -- `focus-link`, `spring-underline`, `article-link`, `pill`, `value`,
`jump-target`, `selectable`. Measured across the site's markup before any of them moved, 426 of
1601 class tokens were these rather than Tailwind utilities, which is the vocabulary announcing
itself.

**One of them has been taken, and which half went where is the part to be exact about.**
`quiet-control` is now `surfaces.quietControl` and the class is deleted. It went first because its
users had already measured the defect this entry describes: seven components kept it and the ones
that needed a variant wrote its declarations out instead, a class being the one kind of name that
cannot be specialized. It was also a mixed rule, so the move split it -- the appearance to the
recipe, and the box, the alignment, the padding and the negative inline margin to Tailwind
utilities at each of the seven call sites. Nothing else moved with it. The prose names --
`focus-link`, `spring-underline`, `article-link`, `jump-target` -- have since left `utilities.css`
for `@canmi/kit/tokens/interaction.css` and `libs/prose/src/prose.css`, still hand-written, because
the CMS draws the same prose; see web's `spec/architecture/css/layers.md`.
`@canmi/ui/primitives` is untouched and still deferred, for the reason below rather than for want of a
spelling.

`@canmi/ui/primitives` is the interesting half. It is CSS rather than TypeScript for one reason: a
Svelte application and a plain-TypeScript application both consume it, and CSS was the only thing
both could read. StyleX removes that constraint, so the package could become typed and composable
for both consumers at once. That is the strongest single argument for this whole arrangement and it
is deliberately not being taken during the migration, because it changes a boundary two
applications depend on rather than the inside of one component.
