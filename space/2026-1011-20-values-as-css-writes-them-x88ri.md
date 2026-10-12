---
name: 'Values as CSS Writes Them'
title: A specimen's value is written as CSS writes it, in the terms it was chosen in, worked out from the style's files as the page is rendered
status: landed
supersedes: []
blocked-by: []
commits: [web@40f37d8a, web@7550e21c]
references: []
---

## Question

The materials page writes each gray's value under its swatch. In what form, in which terms, how is
it set, and how does it sit under the swatch's name?

## Options

- **Form**: a label and a number on two lines, `text` over `4.9%`; a function with commas,
  `oklch(1, 0, 0)`; or the CSS itself, `oklch(1 0 0 / 4.9%)`.
- **Terms**: oklch for every value; or the terms the value was chosen in, a ground by its
  lightness and a rank or a border as the text's color and how much of it is laid.
- **Source**: read off the page after it is painted; or worked out from the style's own
  stylesheets where the server renders, in the theme the reader's cookie names.
- **Spacing**: the mono's spaces whole; or each space half a cell, the other glyphs kept whole.

## Evidence

`text` over a percentage left the reader asking whose percentage it was; the text's color written
out, `oklch(1 0 0 / ...)`, answers it in the value itself. A value read after the paint was blank in
the first frame and flashed in. A whole mono space spread `oklch(1 0 0 / 4.9%)` wide enough to read
as separate words.

## Decision

The CSS itself, in its own terms, from the files, with half spaces.

- **A value is written as CSS writes it**, `oklch(0.145 0 0)`, `oklch(1 0 0 / 4.9%)`, and a press
  still copies the token, `var(--token)`, which is what code reads.
- **In the terms it was chosen in**: a ground by its oklch, a rank or a border as the text's color
  and its alpha, since that is what each is made of.
- **Worked out from the style's own stylesheets as the server renders**, in the theme the reader's
  cookie names, so it is there in the first frame and follows the theme with no measuring; a first
  visit, before the cookie is written, fills it in once the theme is known.
- **Each space is half a mono cell**, `word-spacing: -0.5ch`, the other glyphs whole, and **the
  value stands closer to the name above it than the name to the swatch**.
