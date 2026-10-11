---
name: The Unread Palette Cookie
title: 'The theme script still reads a palette cookie that paints nothing, and whether it stops is open'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`themeScript` adds `nord` or `contrast` as a class when the `palette` cookie holds one, nothing
writes the cookie, and no stylesheet answers either class -- lib's `spec/kit/theme.md`. Whether the
script stops reading it, or a palette control and its stylesheets are built to answer it, is not
decided.
