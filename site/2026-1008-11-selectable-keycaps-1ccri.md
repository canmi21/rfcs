---
name: Selectable Keycaps
title: "The search dialog's keycaps can be selected"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The `kbd` hints in the dialog's footer (`apps/site/src/lib/search/dialog.svelte:462-471`) and its
result buttons (`:418`) are selectable, the plainest break of "a control never is": a drag across
the hint row takes `↑↓ move ↵ open esc close` as text. The group titles (`:~405`) are a judgment
call, a title a reader might quote.

**What deciding it would cost.** `select-none` on the hint row and the rows, kept off the group
titles if they count as quotable.
