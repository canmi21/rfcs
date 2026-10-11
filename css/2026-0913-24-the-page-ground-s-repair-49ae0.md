---
name: "The Page Ground's Repair"
title: 'The page ground is now one name, and the repair it is standing in for is one line on `body`'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Seven components wrote `background-color: var(--color-page)` with `color: var(--color-text)`, and
in five of them those two were the whole style object. They read
`surfaces.page` in `@canmi/kit` now, which removes the duplication and leaves
the question underneath it untouched.

The question is why a route declares the ground at all. Every one of the thirteen addresses puts
the pair on its own `<main>`, so the color is stated once per route rather than once for the
document, and a route that forgot it would render on whatever `body` happens to be. One declaration
on `body` would say it once for the site, and the routes would say nothing.

**It was not taken here because it is a different kind of change.** Naming seven identical copies
moves no declaration between elements and renders identically, which is what the gates can prove.
Moving the pair to `body` changes which element carries it: the ground would paint the whole
viewport rather than a `min-h-screen` box, `color` would inherit from one level further up, and
anything that reads a computed color off `<main>` would read an inherited value instead of a
declared one. None of that is visible in a stylesheet diff and all of it wants the capture.

What deciding it costs is that capture plus a reading of what still needs the pair locally. The
homepage is the one that would not simply lose its key -- it carries `user-select` in the same
object for a reason spec/architecture/css/layers.md records -- so the answer is not uniform even if the
color moves.
