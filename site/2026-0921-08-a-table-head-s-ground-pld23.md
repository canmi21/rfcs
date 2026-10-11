---
name: "A Table Head's Ground"
title: A table head wants a ground that stays the darker one in both themes
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

A Markdown table draws two grounds, and the reading the head is supposed to carry is a band set
behind its rows. That only happens while the head is the deeper of the two, and the palette cannot
promise it -- web's `spec/styling/surfaces.md`, "A mirrored pair cannot keep one of its
members the darker one". With head `paper` the band is right in dark and inverted in light; with
head `paper-hover` it is right in light and inverted in dark. The current assignment is the second,
chosen by looking at both and preferring the light half, not by an argument that settles it.

Three repairs were tried against the running site and each one is a trade rather than a fix.
Swapping the component's two tokens, which is what the current assignment is, moves the inversion
from light to dark rather than removing it. Swapping the palette's
two light values makes both tables right and costs the homepage thumbnail, which stops being a
white sheet and becomes a gray one on a lighter page, along with every card and the light hover
feedback. Giving the head a border instead of a ground was not built; it changes what the block is
rather than which color it takes, so it belongs to whoever decides the table's shape.

Deciding it costs a token pair. A band that must always recede needs two values of its own that do
not mirror -- roughly 0.962 and 1.000 in light, 0.157 and 0.213 in dark, which is the existing pair
with the two themes crossed over. That is a fourth and fifth entry in a palette whose argument is
that it has one home for a color, and it is worth spending only if a second band turns up. So far
the table is the only one.
