---
name: Engaged Borders Spelled Twice
title: 'The border strengthens when a control is engaged, and the site spells that two ways'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `apps/site/src/styles/utilities.css` writes
`.focus-input-shell:has(.focus-input:focus) { border-color: var(--color-border-strong) }`, which
reaches a wrapper from a focused descendant. Three components write the same idea as a conditional
value on the element itself -- `borderColor` from `var(--color-border)` to
`var(--color-border-strong)` on `:hover` and on `:focus-visible` -- in
web's `libs/prose/src/blocks/github.svelte`'s card,
web's `libs/prose/src/blocks/twitter.svelte`'s card and
web's `apps/site/src/lib/support/support.svelte`'s action.

Neither is wrong and the two are not interchangeable. The `:has()` form is the only one available
to it, because the focused element and the bordered element are different elements and a StyleX
condition reaches neither from the other. It also answers `:focus` rather than `:focus-visible` on
purpose -- web's `spec/styling/focus.md` argues that a pointer focus should match the shell
while only a keyboard adds the ring -- so the two differ in what they answer as well as in how.

What is unrecorded is that one idea has two spellings and nothing says which applies when. Deciding
it is either a rule that an element bordering itself writes the conditional value while an element
bordered by a descendant's state needs a selector, or the recognition that the second was never a
choice and only the writing down is missing.
