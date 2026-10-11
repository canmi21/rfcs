---
name: A Data Attribute on Itself
title: A data attribute on the element itself is a condition the visual layer cannot state
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `apps/site/src/lib/support/support.svelte` draws its like pill dark once the
reader has liked: `.like[data-liked='true']:is(:hover, :focus-visible)` sets a border color, a
background and a text color. All three are visual, the element is one the component renders
itself, and no descendant or ancestor is involved -- so by every test in
web's `spec/architecture/css/layers.md` the declarations belong in the visual layer. StyleX
cannot hold them. Its conditions are pseudo-classes, pseudo-elements and at-rules; an attribute
selector is not among them, and the attribute is the whole of what distinguishes this state.

So the pill's resting surface and its hover moved and its liked hover stayed, and one control's
appearance is now written in two layers with nothing in either saying the other exists. It works
only because a scoped rule is unlayered and therefore outranks the visual layer for the properties
they share, which is the accident web's `spec/architecture/css/layers.md` already declines to
promise. The same shape is in web's `libs/prose/src/components/preview.svelte`,
where `[data-starting-style]` and `[data-ending-style]` carry the opening and closing opacities
that Bits UI drives.

This is not the ancestor entry above wearing different clothes. There the state is held by another
element and Tailwind spells the pair `group` and `group-hover:`, so the two halves are in one layer
and the question is how to split them. Here the state is the element's own, Tailwind writes it as a
`data-[...]` variant, and the question is that one of the two layers that could hold a visual
declaration cannot hold this one at all. Deciding it is either that half of the markup moving to
Tailwind's variants, or a rule that a property with an attribute-conditioned value keeps every one
of its values in the selector layer.
