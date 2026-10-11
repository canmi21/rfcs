---
name: Geometry by Cascade
title: Geometry is derived through a cascade of custom properties
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`--rail-width`, `--rail-hold`, `--rail-left-max`, `--rail-icon-overhang`, `--pill-height`,
`--pill-radius`, `--pill-overhang`: a length is declared on an ancestor and the elements below it
compute from it. Roughly a third of `utilities.css` was this before it dissolved -- the rail's lengths
are `libs/prose/src/rail.css` now -- and the newsletter's pill is the same shape at component scale.

It works, it is readable, and it is the pattern the visual layer is least able to hold: StyleX's
model is that everything on an element comes from a class on that element. The question this file
is holding is not whether the arithmetic is right -- web's `spec/styling/lengths.md` argues
each number at length -- but whether the derivation belongs in the selector layer, in the visual
layer as a composed style, or somewhere it currently is not.
