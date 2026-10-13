---
name: 'Polish the Registry Pages'
title: "What the registry holds is drawn as it should be: a node's and an app's Spec, a node's Drift, Accounts and any kind under /kinds"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The console was built for what it does ahead of how it looks, decided with the author on 2026-10-13: its problem was what it lacked, not its look. web's `spec/console/polish.md` holds the rule: an unpolished surface uses the design system as it comes, writes no style of its own, and is wrapped in `<Unpolished>` naming this RFC. The UI/UX work polishes it later.

## Decision

- **What waits for polish here**: a node's and an app's `Spec`, read and edited from the registry's catalog; a node's `Drift`; the Accounts pages and their levels (Domains left on 2026-10-13, RFC 32hd9); and the pages any kind gets under `/kinds/<plural>`.
- **Done when** each is styled as the polished overview is, its `<Unpolished>` mark taken off, and
  this RFC landed with the commits that did it.
