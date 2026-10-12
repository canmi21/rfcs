---
name: 'Unbuilt Is Shown, Not Pressed'
title: What a reader would press but is not built yet stays in its place, named and unlinked, and the pointer over it shows not-allowed
status: landed
supersedes: []
blocked-by: []
commits: [web@83073fb8, web@d094899e, web@2a4fa7ce]
references: []
---

## Question

Some of what a reader would press leads to nothing built yet: a layer with no page in the top bar,
a page of a category not drawn, an RFC cited while the RFCs' pages are reached nowhere. Is it
hidden, left as it is with nothing to press, or shown and refused?

## Options

- Hidden until it is built.
- Shown as plain words, with nothing to press and nothing said.
- Shown in its place, named and not linked, the pointer over it showing `not-allowed`.

## Evidence

Hidden, a category looked smaller than it will be, and the top bar changed shape as each layer
arrived. Left as plain words where a link had stood, an RFC citation read as a link that did nothing
when pressed, with no sign why. In the sidebar and the top bar, an entry named in the subtle tone
with the pointer showing `not-allowed` already told the reader, in place, that it is coming and not
here yet.

## Decision

- **What a reader would press but is not built yet is shown in its place, named and not linked**,
  never hidden and never left silent.
- **The pointer over it shows `not-allowed`**, the one hint every such place gives.
- **Among links it takes the subtle tone**, as the sidebar's and the top bar's entries do; standing
  alone, as an RFC citation does, it keeps its own.
- **It lifts when what it leads to is built**: the entry becomes a link, and the hint goes with it.
