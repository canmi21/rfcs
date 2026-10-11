---
name: "Tokei's Own Palette"
title: 'Tokei draws from a palette of its own, and it is the third one'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `spec/architecture/css/authoring.md` exempts two component-local palettes from the rule that
a color is the token layer's -- Cargo's and Mermaid's -- argued in
web's `spec/styling/controls.md` and web's `spec/styling/blocks.md`.
web's `libs/prose/src/blocks/tokei/tokei.svelte` has a third that neither file names:
a color per language from `langColor`, three hexes in `FUNCTION_COLORS` for code, comments and
blanks, two `rgba()` literals inline on the tile's completion bar, and two whites inking a tile's
label over whatever color the language happens to be.

The whites are the ones the migration had to rule on, because they are `fill` on an element this
component renders and `fill` is visual by the same test that moved `fill-current` in
web's `libs/prose/src/icons.svelte`. They stayed, for the reason the link card's
black and white stayed two entries above: a color that no token declares cannot be stated in the
layer whose one structural guarantee is that a color is a token variable. So `.tile-name` and
`.tile-size` are now a `fill` in the selector layer and a size and a weight in the visual one.

What is unresolved is smaller than the palette and larger than this component. Two exemptions
written as a list have a third member nobody added, and the test that admits them -- artwork rather
than interface -- is not written anywhere. Deciding it is either naming the exemption instead of
enumerating it, or a home for a chart's palette that is neither the token layer nor a component.
