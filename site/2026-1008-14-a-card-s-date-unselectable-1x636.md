---
name: "A Card's Date Unselectable"
title: "A card's date cannot be selected, while the article's own date can"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The home list's card leaves its `<time>` unselectable on purpose
(`libs/prose/src/card.svelte:79-91`, its comment says so), while the article page reopens the same
date with `select-text` (`apps/site/src/lib/article/article.svelte:440`). A date is something a
reader quotes, so one of the two is wrong, and the comment's reason has to be weighed against that.
The meta row's word and read counts (`article.svelte:441-466`) are unselectable, which the rules are
silent on.

**What deciding it would cost.** Which way the card goes, and whether counts are quotable.
