---
name: An Option Styled and Scoped
title: 'An option with a style directive the stylesheet also scopes writes style before class, where Svelte writes class first'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

**An `<option>` with a `style:` directive the stylesheet also scopes.** `attributes()` appends the
scoping class and then the directives' `style` to an option's attributes, and the walk plants the
directives' anchor as a `style` attribute of the option's own, so the render writes `style` before
`class` where Svelte writes `class` before `style`. A build meets it only where a stylesheet selects
an option; under the dev server every element is scoped. Found beside the `selected` decision, which
had the same order wrong and is closed (`lastSelected` in `pkgs/skeleton/src/render.ts`); moving the
anchor after what `attributes()` appends is the same move.
