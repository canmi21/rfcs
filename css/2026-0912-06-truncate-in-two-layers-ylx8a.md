---
name: Truncate in Two Layers
title: '`truncate` is one utility and two layers'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`truncate` is `overflow: hidden` plus `text-overflow: ellipsis` plus `white-space: nowrap`. The
first decides how large the box is and the other two decide how the text looks, so under
web's `spec/architecture/css/layers.md` the utility straddles the boundary and no third of it
can move without settling where the other two go. The package page carries eight of them and six
other components carry nine more.

Splitting it costs three declarations across two layers at every site, and the markup stops saying
that the element ellipsises -- which is the argument for utilities, applied against itself.
Keeping it whole leaves `text-overflow` and `white-space` as the only typography the visual layer
does not own. The decision is about compound utilities in general rather than about this page,
and `line-clamp` has the same shape.
