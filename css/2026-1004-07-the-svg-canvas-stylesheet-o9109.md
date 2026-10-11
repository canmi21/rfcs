---
name: The SVG Canvas Stylesheet
title: '`@canmi/ui/svg-canvas` is a 527-line global stylesheet'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

It styles generated SVG, so it is the selector layer by the definition in
web's `spec/architecture/css/layers.md`. It is also five times the size of every other file in
that layer, unscoped, and loaded by whoever imports the library. Whether a drawing's appearance is
the drawing's or the site's is the question underneath it.
