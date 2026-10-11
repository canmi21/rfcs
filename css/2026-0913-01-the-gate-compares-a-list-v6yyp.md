---
name: The Gate Compares a List
title: 'The gate compares a list, and a list is not a test'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`cursor`, `pointer-events` and `user-select` are settled as visual in
web's `spec/architecture/css/layers.md`, so the article shell's
`.article-rail, .meta { user-select: none }` moved into the visual layer with everything else. Of
the three, only `cursor` is in the seventy-two properties the migration's snapshot compares.
`user-select` is not, `-webkit-user-select` is not, and neither is `pointer-events`.

Measured while migrating web's `apps/site/src/lib/article/article.svelte`: the style
was applied to the rail and not to the metadata row beside it, so the row silently became
selectable again -- and the diff over 120 snapshots was empty. It was found by reading the computed
style in a browser afterwards. A static snapshot has a second reason to miss it, which is that
nothing in the harness drags across an element.

The finding is not that one name is missing from a list. It is that the list was written from the
properties a migration was expected to move, while what a migration was allowed to move was decided
by a test, and a list and a test drift the first time somebody applies the test honestly. That
observation is now web's `spec/architecture/css/layers.md`'s own argument for
giving the test up: the enumeration there is the rule, and the test is only how the enumeration
grows.

**The first of those has since been taken and the second is still open.** The list went from
seventy-three properties to ninety-eight, audited against what the site actually declares rather
than against what a migration was expected to touch, and `border-*-style` was found compared on two
edges where width and color were compared on four. What is still true is that a list maintained by
hand drifts from a test applied honestly, and that nothing reports the drift. Two properties remain
knowingly outside it -- `transition-duration` and `-delay`, which the harness freezes so that two
runs agree, which is the same act that makes them unreadable -- and no custom property is compared
at all, so a utility leaving the markup takes its private variables with it unnoticed.
