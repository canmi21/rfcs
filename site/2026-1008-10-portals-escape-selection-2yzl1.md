---
name: Portals Escape Selection
title: "A portalled surface escapes every page's choice of what can be selected"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Bits UI portals the search dialog, the modals, the translator-note popover and the menus to
`<body>`, outside any `<main>`, so the home page's `select-none` reaches none of them:
`apps/site/src/lib/search/dialog.svelte`, `apps/site/src/lib/components/modal.svelte:92-121`,
`libs/prose/src/body.svelte:360-370`, `apps/site/src/lib/components/menu-content.svelte:48`. Their
controls -- the result rows, a modal's close button, the popover's -- are selectable on every page,
while the same kind of control on the home page is not.

**What deciding it would cost.** Rules on the portalled surfaces themselves, since no ancestor of
theirs is the page's.
