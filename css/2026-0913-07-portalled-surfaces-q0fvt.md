---
name: Portalled Surfaces
title: "A portalled surface is out of Svelte's reach and not out of the visual layer's"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The entry above on floating surfaces says Bits UI portals them out of the component tree and they
are therefore reached with `:global`. Migrating
web's `libs/prose/src/components/preview.svelte` sharpens that: the portal is what
puts them out of the _selector_ layer's reach, and it puts them out of nothing else.

The component writes `class="preview-ground fixed inset-0 z-50"` on `Dialog.Overlay`, and Bits UI
puts that string on the element it renders in the portal. Svelte does not add its scoping class to
a component's `class` prop, which is the whole reason the rules below are `:global`. A StyleX class
is a plain global class name and travels the same prop, so the visual layer could reach all three
of these surfaces -- the ground, the stage and the close -- without a selector of any kind.

It was not taken, and the reasons are worth recording because they are not the portal. The ground's
`#000` and the close's wash and hairline are literals rather than tokens, argued as such in
web's `spec/styling/blocks.md`, so moving them would put the site's only unthemed colors
into the layer whose one structural guarantee is that a color is a token variable. And every one of
those rules has a second half conditioned on a data attribute, which is the entry above: the
transitions would move and the opacities they animate could not. Three rules split across two
layers, for nothing gained.

So the decision here is downstream of the other two rather than its own. What is new is only that
the constraint on these surfaces has been misattributed: they are unreachable by one layer, not by
two.
