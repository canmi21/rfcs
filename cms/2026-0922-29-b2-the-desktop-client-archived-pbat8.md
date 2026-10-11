---
name: 'B2: The Desktop Client Archived'
title: 'The desktop client leaves the repository for one of its own, out of the workspace and out of check'
status: landed
supersedes: []
blocked-by: [ex27b]
commits: [web@d93ec8b6, web@ed7beff2]
references: []
---

## Question

The desktop client built nothing any more once `local` answered HTTP, and still sat in the workspace
and in `check`. B2 of web's milestones.

## Decision

- **Archived**: its source moved to a repository of its own, out of the workspace and out of `check`.
- **What its window settled about an interface is kept** in `local`'s spec, and what only a Tauri
  window could inherit dropped.
