---
name: "The Console's Head Names Marks"
title: "The console's head names its marks as objects resolved at render, as the site's does"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The console's `app.html` links `/favicon.*`, while the site names its marks by the objects they
resolve to.

## Decision

- **Resolved at render by its root layout's server and named as objects**, its `app.html` no longer
  linking `/favicon.*` -- web's `spec/architecture/console.md`, and platform's
  `spec/architecture/delivery.md`, "A head names its marks by the objects they resolve to at
  render".
