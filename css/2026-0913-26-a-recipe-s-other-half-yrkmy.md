---
name: "A Recipe's Other Half"
title: "A recipe's other half is a convention and nothing checks that a call site kept it"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`surfaces.quietControl` is the visual half of what was one CSS rule. The other half -- `-mx-1
inline-flex items-center px-1 py-0.5` -- is Tailwind utilities that each of the seven call sites
carries in its own markup, because that half is layout and the markup is where layout lives. The
split is what web's `spec/architecture/css/layers.md` requires and the halves are in the right
places.

What went with it is that the rule could not be half-applied and the recipe can. Composing
`surfaces.quietControl` and forgetting the utilities gives a control the right colors in the
wrong box, and nothing says so: the recipe is a valid style object on its own, so the compiler is
content, and the stylesheet is unchanged either way because those utilities were already emitted
for some other element on the page. Every gate this repository has is blind to it.

This is the first surface whose two halves live in two layers, so it is the first time the
question arises. It arises again for every mixed rule still in `utilities.css`, and the answers
available
are not obviously equal: a comment on the recipe, a second export holding the class string, or the
recognition that a surface wanting both halves is one a component should be rather than one a
markup string composes.
