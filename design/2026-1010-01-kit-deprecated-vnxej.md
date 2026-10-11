---
name: Kit Deprecated
title: 'Every app on the kit moves to the design system, and the kit is deprecated once nothing reads it'
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`@canmi/kit` is frozen -- lib's `spec/design/styles.md`, "Kit is frozen" -- and `@canmi/design` is
growing in its place. What becomes of the kit, and when?

## Decision

- **Each app still on `@canmi/kit` moves to `@canmi/design`**: its theme script, palettes, tokens,
  motion and behavior taken over as the new package grows them.
- **The kit is marked deprecated on npm once nothing reads it**, and not before.
