---
name: 'Polish the Fleet Pages'
title: "The fleet's pages are drawn as they should be: Nodes, Apps, Deployments, Events, and every page of a node and an app"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The console was built for what it does ahead of how it looks, decided with the author on 2026-10-13: its problem was what it lacked, not its look. web's `spec/console/polish.md` holds the rule: an unpolished surface uses the design system as it comes, writes no style of its own, and is wrapped in `<Unpolished>` naming this RFC. The UI/UX work polishes it later.

## Decision

- **What waits for polish here**: the Nodes, Apps, Deployments and Events pages, and the pages of a node -- Overview, Apps, Events, Disk -- and of an app -- Overview, Usage, Deploy history.
- **Done when** each is styled as the polished overview is, its `<Unpolished>` mark taken off, and
  this RFC landed with the commits that did it.
