---
name: Keyframes Hold Resting Values
title: A keyframe holds the resting values the visual layer now owns
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Svelte rewrites a keyframe's name to a scoped one and rewrites the `animation` properties in the
same block to match. Nothing outside that block can name it: an `animation-name` written in the
visual layer points at a keyframe that does not exist. So the newsletter's eight keyframes stay in
the selector layer, and so do the eleven classes whose whole content is an `animation` naming one,
even though web's `spec/architecture/css/layers.md` lists motion as the visual layer's subject.

The part that is a finding rather than a consequence is what the keyframes contain. `cool`'s `to`
block is `background-color: var(--color-paper-hover)` and `color: var(--color-text-soft)`, which is
the chip's resting appearance character for character -- and that resting appearance is now a
StyleX style at the head of the same file. The two were adjacent before the migration and are now
in two layers and two halves of the file with nothing tying them together, so changing the chip's
ink without changing the keyframe lands the animation somewhere the element does not rest. `spend`
and `.spent` are the same shape in `visibility`.

Deciding it is either `stylex.keyframes`, which moves the interpolation into the visual layer and
leaves the Svelte block only what genuinely needs a selector, or a convention that a keyframe's
endpoint and the resting declaration read one custom property rather than two literals.

The article shell qualifies the first sentence of this entry. Its one keyframe is written
`@keyframes -global-note-return-marker`, which is Svelte's opt-out from scoping: the name reaches
the document as `note-return-marker` and any layer could write an `animation-name` pointing at it.
It stays in the selector layer anyway, because what it animates is a note marker the markdown
compiler produced and no class of ours is on that element. So one shape hides two constraints --
sometimes the keyframe's name cannot be reached from the visual layer, and sometimes the name can
be reached and its subject cannot -- and only the first of them is what `stylex.keyframes` repairs.
