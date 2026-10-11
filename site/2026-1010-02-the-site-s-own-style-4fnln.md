---
name: "The Site's Own Style"
title: The site keeps a style no design system holds
status: draft
supersedes: []
blocked-by: [p6r5m]
commits: []
references: []
---

## Question

web's `spec/architecture/space.md`, "The design system: its layers, and a
style across them", has the console and space on one contract of semantic colors and one family of
components. The site's style is its own and only it uses it -- its components in
`apps/site/src/lib/components/` and `@canmi/ui/primitives`, which draws and so is a component
family rather than primitives. Moving it on means its colors given as values of the contract and
its components standing on the shared primitives, each checked for visual regression.

**What deciding it would cost.** A pass over every page for regressions, which there is no time
for while the console and space are being moved.
