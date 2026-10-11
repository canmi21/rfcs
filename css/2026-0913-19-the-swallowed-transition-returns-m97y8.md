---
name: The Swallowed Transition Returns
title: The swallowed transition returns the moment the transition is given a name
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The entry above on `.spring-underline` records that it sits outside every `@layer`, that its
`transition` shorthand takes all four longhands, and that the ten-property `transition-colors` list
therefore never reaches an element carrying the class. That is a fact about today, where the list
is written out at each site.

It becomes a trap the moment the list is written once. Six components carry the whole of one soft
control -- a color going from `var(--color-text-soft)` to `var(--color-text-strong)` under `@media
(hover: hover) :hover` and under `:focus-visible`, beside `transition.colors`, `duration.base` and
`easing.inOut` -- and twelve carry the transition pair alone, which is the group most likely to be
named first. A composed style holding those three, applied to an element that also carries
`spring-underline`, is the same swallow as today with one difference that matters: the name says
the element has a transition, and a reader has no reason to check.

So this is not a second instance of the entry above. It is that entry's consequence for the naming
decision the first entry in this file is holding, and the cost of getting it wrong rises rather
than staying flat -- a name is read as a decision, while a repeated literal is read as something
nobody has looked at yet.
