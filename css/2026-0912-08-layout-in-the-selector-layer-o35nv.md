---
name: Layout in the Selector Layer
title: 'Layout sits in the selector layer, in nearly every block that has one'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `spec/architecture/css/migration.md` says a migration moves the visual layer and stops
there, which is why a migrated block comes out smaller and still mixed. `code-block.svelte` is the
first one large enough to show what is left: `position`, `top`, `right`, `z-index`, five
`display`s, four widths, three `overflow`s and a `height`, most of which needs no selector to
reach the element it styles.

Measured across the site at the time: 203 of the 327 rules in the 25 `<style>` blocks named
nothing but a class on an element the component itself renders, and 329 of the declarations inside
them were layout. Under web's `spec/architecture/css/layers.md` that is the markup's, written as
utilities on the element, and it is sitting in the one layer that exists to hold what the other
two cannot address.

Moving it is a second pass rather than a corollary of the first. It is hand-authored geometry
rewritten as utilities -- `min-width: 1.5rem` as `min-w-6`, `padding-inline: 0.25rem` as `px-1` --
and the part of it a data attribute gates, such as the code block's `[data-phase='collapsed']`,
can only follow through Tailwind's `data-[...]` variants, which one component on the site uses
today. What it buys is a block that finally means one thing.
