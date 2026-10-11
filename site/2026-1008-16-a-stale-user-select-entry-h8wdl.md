---
name: A Stale User-Select Entry
title: The user-select entry in the CSS issues describes CSS that is now a class
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

RFC v6yyp, records `.article-rail,
.meta { user-select: none }` moving into the visual layer; both are now Tailwind's `select-none` in
the markup (`libs/prose/src/shell.svelte:50`, `apps/site/src/lib/article/article.svelte:435`), so
the entry describes a state the code no longer has. Its point -- that the migration's snapshot does
not compare `user-select` -- still stands.

**What deciding it would cost.** Rewriting that entry against the code as it is, or closing it if
the point moves to where the selectability rule lands.
