---
name: Only-Conditional Styles
title: A style that is only ever conditional cannot be merged into a class attribute
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`stylex.attrs()` builds its result by omission. Read in `@stylexjs/stylex` 0.19.0: `props()` adds
`className` only when the resolved string is neither null nor empty, and `attrs()` adds `class` only
when `props()` returned one, so a call whose every argument is switched off returns `{}`. Written as
`stylex.attrs(cond && styles.x).class` that is `undefined`, and a Svelte class attribute
interpolating it renders those nine characters as a class token beside the real ones.

It bites once so far. link-card's corner arrow takes `mix-blend-difference` only when the block
declares no tone, and there is no unconditional declaration left on that element to hold the
attribute open, so the utility stayed in the markup. The one other conditional on the site --
web's `apps/site/src/lib/locale/switcher.svelte`'s caret -- passes an unconditional
style first and never sees it.

Three repairs, and they are not equivalent. A `?? ''` at each site is one more thing to remember at
exactly the place web's `spec/architecture/css/authoring.md` already says a rule cannot be checked.
A helper that merges class strings is the repair that file proposes for `attrs` replacing `class`,
and it would absorb this case for free. Writing the off state as its own style so that something
always resolves adds a declaration the markup never had, which a migration may not do. The middle
one belongs with the `attrs` hazard rather than beside it.
