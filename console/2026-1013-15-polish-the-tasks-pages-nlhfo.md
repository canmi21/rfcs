---
name: 'Polish the Tasks Pages'
title: "The ledger's tasks are drawn as they should be: the Tasks list and a task's timeline"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The console was built for what it does ahead of how it looks, decided with the author on 2026-10-13: its problem was what it lacked, not its look. web's `spec/console/polish.md` holds the rule: an unpolished surface uses the design system as it comes, writes no style of its own, and is wrapped in `<Unpolished>` naming this RFC. The UI/UX work polishes it later.

## Decision

- **What waits for polish here**: the Tasks page, the ledger's tasks newest first, and a task's page, its events in order.
- **Done when** each is styled as the polished overview is, its `<Unpolished>` mark taken off, and
  this RFC landed with the commits that did it.
