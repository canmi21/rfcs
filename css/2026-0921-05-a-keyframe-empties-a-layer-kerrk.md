---
name: A Keyframe Empties a Layer
title: 'A keyframe in the vocabulary empties a StyleX layer, and no gate sees it'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`stylex.keyframes` works: probed against this repository's own `@stylexjs/babel-plugin@0.19.0`,
eight shapes compiled, including `var()` inside the frames, every `animation-*` longhand with a
custom-property value, and a call inside a function body -- which matters, because a Svelte
`<script>` is the instance function rather than module scope.

**What it costs is the layer below it.** `processStylexRules` groups rules by
`Math.floor(priority / 1000)` and emits a group as `@layer priorityN` only when the first rule's
priority after sorting is above zero. Keyframes are priority 0 and custom properties are priority
1, so both floor to the same group; a keyframe sorts first, and the whole group ships unlayered.
Measured: with two keyframes added, the built sheet carried an empty `@layer priority1;` and, at
its end, those keyframes beside `.xg5cpi7{--wash:10%}` and the three
`:focus-visible{--focus-ring-*}` rules with no layer at all.

Nothing rendered differently -- each of those custom properties has one declarer sheet-wide. What
went is the discipline: unlayered outranks every layer including the escape hatch, so a custom
property StyleX writes from then on silently outranks a scoped rule writing the same one. Three of
the four de-layered rules are the focus ring's own suppression, whose fix turns on the sentence
that a custom property obeys cascade layers like any other property.

**No gate sees it.** `css-layers.ts` reads only layered selectors, so it passed and would go on
passing. `useCSSLayers` takes `{before, after, prefix}` and none of them reaches the `pri > 0`
test, so there is no configuration out.

**Second blocker, independent of the first.** `animationName: breathe` is an Identifier, and
`apps/site/scripts/css-source.ts` refuses to read what it cannot resolve -- "This scan is
measuring less than the tree holds, so its count is not a measurement" -- which reddens all three
source gates. There is no literal spelling of a keyframes reference, so the scan has to learn the
shape before any keyframe can move.

Until both are answered, the thirteen keyframe-bound scoped rules stay in the escape hatch and so
do the eight keyframes a survey found otherwise movable. That is a larger floor than the survey
concluded, and the reason is a defect rather than a capability. A measured-working mermaid sits in
the session scratchpad; it was reverted rather than landed red.
