---
name: Ancestor State by Marker
title: Ancestor state reaches the visual layer only through a marker nobody owns
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`section.svelte` reveals its anchor button while the pointer is over the heading. Tailwind spells
that `group` plus `group-hover:`, and the two halves cannot be split across layers -- StyleX
outranks Tailwind's utilities, so a resting `opacity: 0` in the visual layer would win over a
`group-hover:opacity-100` left in the markup and the control would never appear. Moving the
resting opacity therefore means moving the hovered one.

StyleX's own answer is `stylex.defineMarker()`, and it is refused here twice: first with `the
return value of defineMarker() must be bound to a named export`, then, once exported, with
`unable to generate hash for defineMarker(). Check that the file has a valid extension and that
unstable_moduleResolution is configured`. A `.svelte` file is not an extension that API will hash.
The component uses `stylex.defaultMarker()` instead, which compiles to the literal class
`.x-default-marker` -- one name, shared by every component that calls it, so two of them nested
would have the outer one's hover reveal the inner one's control.

The second half of that error message named a blocker that is gone, and saying so here is a
correction: `unstable_moduleResolution` **is** set in the vite plugin -- see
web's `spec/architecture/css/layers.md`, "The module resolution is stated rather than defaulted,
because it is what makes `#lib` reachable" -- and `@canmi/kit/tokens`'s `vocabulary.stylex.ts` is already the
`.stylex.ts` home a marker would live in. Neither is a cost this decision still has to pay, and
the entry priced both.

What keeps `defaultMarker()` in place is the extension refusal above plus a type mismatch the
component records at web's `libs/prose/src/section.svelte`: `when.ancestor`
is called with no explicit marker, because its parameter is branded for a `defineMarker()` symbol
and the default marker is branded as itself, so handing the default one over would not type check.
A named marker in a `.stylex.ts` file is what that parameter wants, and moving to one is now a
change inside two files rather than a boundary question.
