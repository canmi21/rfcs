---
name: The Extraction Threshold
title: The extraction threshold counts one layer and the vocabulary lives in three
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `spec/architecture/css/extraction.md` admits a value to the vocabulary at three components,
and the count is taken over the thirty-five `stylex.create` blocks. The named surfaces are not all
in those blocks. web's `apps/site/src/styles/utilities.css` holds fifteen classes,
`@canmi/ui/primitives` holds eight across eleven selectors, and
`app.css` holds four more -- `.pill-metrics`, `.pill`, `.value` and `.value-cell` -- so a surface
can have two instances in the visual layer, sit below the bar, and already be written a third time
in a stylesheet.

Two of those three files sit above the visual layer, which makes the miscount worse than an
accounting error. Measured on `app.css`: it opens with four imports,
`@canmi/ui/primitives`'s stylesheet third and
web's `apps/site/src/styles/utilities.css` fourth, and neither is inside an `@layer`.
`utilities.css` layers part of itself and `@canmi/ui/primitives` layers none of itself, so every rule in
the second is unlayered and, by web's `spec/architecture/css/layers.md`'s count, nineteen of the
twenty-seven selectors in the first are too -- and an unlayered rule outranks every layered one.
That is the fourth participant that file already records for `utilities.css`, now confirmed by
measurement for `@canmi/ui/primitives` rather than inferred from the import form. So a surface named
in the visual layer and also written in one of those two is not merely counted twice: on an
element carrying both, the copy that renders is the one the count did not see.

Three cases, measured. **The dashed leader** is `border-top: 1px dashed var(--color-border-strong)`
in `.article-preview-leader::before` and the same three declarations in `leader` on two license
pages: two in the counted layer, three in the site. **The bordered paper surface** --
`background-color: var(--color-paper)` with `border-style: solid` and `border-color:
var(--color-border)` -- is ten style keys in ten components, plus `.value-cell` in `app.css` and
`.article-preview-thumbnail` in `@canmi/ui/primitives`, each at its own radius: four radii for one
surface across three layers. And **`color: var(--color-text-strong)` with `font-weight: 500`** is
thirteen style keys in twelve components, and the first two declarations of
`.article-preview-title`.

The threshold is not wrong; it is measured over the wrong set. Deciding it costs either a count
that reads all three layers, which means a script parsing CSS as well as TypeScript, or the
admission that the bar is about the visual layer's own repetition and that agreement with a
stylesheet is a separate observation. The two answers differ for the leader, which clears three
across the site and does not clear it inside one layer.
