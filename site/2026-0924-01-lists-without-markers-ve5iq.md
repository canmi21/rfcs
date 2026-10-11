---
name: Lists Without Markers
title: A list on the page has no marker and no indent
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Measured on the preview of a draft holding one, 2026-09-24: `ul` and `ol` compute `list-style-type:
none` with no padding, so a list reads as lines of plain text -- Tailwind's reset takes the markers
away and nothing in web's `libs/prose/src/prose-root.svelte` puts them back. No
article has used a list yet, which is why nobody saw it. The CMS's editor draws a list as its source
until this is decided, rather than inventing a look the page does not have.

**What deciding it would cost.** A rule for `ul`, `ol` and `li` beside the blockquote's in the
prose root, which the editor then draws to match: the marker, the indent, the space between items.
