---
name: A Package Module Changed
title: "A package's module whose state something changes is two values, the render's and the carried bundle's, and is not refused"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

**A package's module whose state something changes.** The render imports each module afresh and
the carried bundle imports it once, so a binding a module changes is two values. It is refused for
a relative module, whose source is read; a package's is the hole, and reading the package's source
the way `carry` already resolves the file is what closes it. seam's `spec/readings.md`, "The
render's module instances are not the artifact's".
