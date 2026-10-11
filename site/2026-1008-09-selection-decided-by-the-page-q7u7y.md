---
name: Selection Decided by the Page
title: 'What can be selected is decided by the page around a component, not by the component'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The console's three rules -- chrome is not selectable, a control never is, what a reader would quote
is -- are web's `spec/console/design.md`, "What can be selected", and the site follows
none of them yet. It sets `select-none` in three places only: the home page's `<main>`
(`apps/site/src/routes/+page.svelte:147`), the article rail (`libs/prose/src/shell.svelte:50`) and
the article's meta row (`apps/site/src/lib/article/article.svelte:435`). Everything else inherits
from those or from nothing, so the answer for one component depends on the page it lands on: the
home page turns selection off and reopens fourteen sentences one at a time, the article page leaves
all of it on but the rail and the row, and the licenses and error pages do nothing. Measured on
2026-10-08 by reading the markup and its ancestors.

**What deciding it would cost.** Each control carrying its own rule, as the console's do, instead of
a page deciding for everything under it; the entries below are the departures that one change would
close, and each is worth a browser check after it.
