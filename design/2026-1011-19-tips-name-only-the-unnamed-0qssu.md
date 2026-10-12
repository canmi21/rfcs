---
name: 'Tips Name Only the Unnamed'
title: A tooltip shows only on a mark its name does not stand beside, a row of them shares its gaps, and its point is placed by the browser
status: landed
supersedes: []
blocked-by: []
commits: [lib@7c593eb7, web@7550e21c]
references: []
---

## Question

The design pages put a tooltip on every swatch. Below the first card each swatch stands over its
own name and value, so its tip said again what the reader had just read. Across a row of tipped
swatches the pointer fell into the gaps between them, and the tip closed and opened as it crossed.
And a tip's point sat at its card's left edge for its first frame before it moved to the middle.
When does a mark get a tip, how does the pointer cross a row of them, and how is the point placed?

## Options

- **When**: a tip on every mark, as a rule of the component; or a tip only where the mark says
  less than its name, a swatch or an icon standing alone.
- **Crossing a row**: the gaps left to the page, so the pointer leaves one mark before it meets the
  next; or each mark taking half of the gap on either side, unseen, so the row has no gap to fall
  into.
- **The point**: placed at half the card's width as measured, which is nothing until the card is
  measured and is rounded to whole pixels after; or placed at the middle by CSS, `50%`.

## Evidence

A tip that repeats the words beside it adds a second reading of one name and covers what is above
it. Measured on the hues page, the point stood 0.2 pixels off its card's middle once settled, and
at the card's left edge in the frame before the card was measured, which a reader sees each time a
tip first opens. Scanned pixel by pixel across a row of swatches whose wells reach half a gap,
every point falls on a swatch and they come in order, none skipped.

## Decision

Only the unnamed, half of each gap, and the middle by CSS.

- **A tip shows on a mark that says less than its name**: a swatch, an icon, with nothing beside
  it. Where the name stands next to what it holds, nothing shows on the pointer, and the mark keeps
  its own action, a press that copies.
- **Marks with tips in a row share the gaps between them**: each takes half of the gap on either
  side as its own pointer area, unseen, so the tip follows the pointer across the row without a
  break.
- **The point stands at the card's middle as the browser places it**, never measured: Point takes
  `along: 'middle'` for it, right from the first frame.
