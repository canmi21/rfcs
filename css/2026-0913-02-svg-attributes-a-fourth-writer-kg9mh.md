---
name: 'SVG Attributes, a Fourth Writer'
title: 'An SVG presentation attribute is a fourth writer, and it sits below every layer'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `libs/prose/src/icons.svelte` draws ten glyphs and two of them ink
themselves twice. Every branch carried `fill-current`, which is visual and moved; `twitter` also
carries `stroke="currentColor"`, `stroke-width="0.7"` and `stroke-linejoin="round"` as SVG
presentation attributes, and `moe` carries those three plus `fill-rule="evenodd"`. All seven decide
how the glyph looks and none of them is in any of the three layers.

A presentation attribute is defeated by every CSS declaration naming the same property, whatever
layer it sits in and whether it is layered at all. So this is the mirror of the first entry in this
file: there a named vocabulary sits above the visual layer with nothing saying it should, here a set
of visual declarations sits below all three with nothing saying it should. Neither is overridden
today, which is why the migration left these alone -- moving `fill` while `stroke` stayed an
attribute changed no pixel, because nothing anywhere sets `stroke` on these elements.

Deciding it costs a behavior change rather than a rendering one: a stroke written in the visual
layer stops being overridable by a caller's utility and starts outranking it, on a glyph the
homepage and the tweet block both render. It also asks whether a path's own description belongs to
the site at all -- `fill-rule` is closer to the artwork than to the interface -- which is the
question `@canmi/ui/svg-canvas` above is already holding.
