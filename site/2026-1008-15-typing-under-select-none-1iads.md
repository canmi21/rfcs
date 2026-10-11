---
name: Typing Under select-none
title: "A field under the home page's `select-none` may not take typing in Safari"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The newsletter's email input sits under the home `<main>`'s `select-none` with nothing reopening it
(`apps/site/src/lib/newsletter/newsletter.svelte`, near `:400`). WebKit has been known to let an
ancestor's `-webkit-user-select: none` stop an input from taking a caret; not tested here, so this
is inferred, and the rule that a field's text is always selectable assumes it is not so.

**What deciding it would cost.** A check in Safari, then `select-text` on fields under a
`select-none` ancestor if it fails.
