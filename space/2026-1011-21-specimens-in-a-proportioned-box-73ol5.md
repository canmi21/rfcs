---
name: 'Specimens in a Proportioned Box'
title: A specimen stands in a box on the page's ground, its row set in a proportion of the room it leaves either side, measured against its own row or its series' widest, its labels hung below
status: landed
supersedes: []
blocked-by: []
commits: [web@51e5ef25, web@c9c3a9a0, web@fd2b7e83]
references: []
---

## Question

A design page shows each specimen on a card. What does the specimen stand on, how much room is left
around it, and how do boxes that sit together, and the names and values under a row, share that
room?

## Options

- **Ground**: the specimen straight on the card; or a box, the card again on the page's own ground,
  holding it.
- **Room**: a fixed padding, `3rem` above and below and `2rem` either side; a fixed height; or a
  proportion of the room the row leaves either side.
- **Measure**: every box against its own row; or a series of related boxes against the widest row
  among them.
- **Labels**: counted in the row the box centers; or hung below it.

## Evidence

A page with nothing under its specimens read as no page at all, and a card set on a card read as a
second sheet laid over the first; the box, being the card again on the page's own ground, sets a
card in it on ground, as it is drawn to stand.

A fixed padding left a short row adrift in a wide box and a wide row cramped in a narrow one. Set as
a share of the room either side, a row stood the same at any width.

On materials, each group measured against its own row stood at its own height: at 1299px,
Background's two tiles left 325px either side, so 163px above and below, against 79px for the groups
of four, and the four cards lost one rhythm. Measured against the longest group, all four stood at
one height, the groups of four at the proportion exactly and the shorter ones wider than it.

With each tile's name and value counted in the row, the tiles sat high in the box and the space
below them was mostly text; hung below the row, the tiles stood centered and the text read as theirs.

## Decision

- **A box, the card again on the page's own ground**, holds the specimen, with `lg` corners inside
  the card's `xl`; a specimen with a shape of its own keeps its proportions in it.
- **The room either side is what the row leaves, never less than 2rem, and the room above and below
  is that times `2`, `1`, `2/3`, `1/2` or `1/3`**, never less than 2rem itself, worked out from the
  card's width as it is. The sides are the measure and are never moved for the proportion.
- **A box is measured against its own row, or against its series' anchor**, the widest row of a
  series of boxes that belong together, parallel groups on one page; boxes merely near each other
  are no series. The material groups are one, anchored on the longest, at `1/2`.
- **Names and values under a row hang below it and are left out of its centering**, pushing the
  foot down only where the box is too short to hold them, never nearer its edge than 1rem.
