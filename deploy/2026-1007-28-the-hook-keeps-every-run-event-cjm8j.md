---
name: The Hook Keeps Every Run Event
title: "The hook keeps every workflow_run event, so the console's queue shows a run before any node sees it"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The console's queue shows a run only once a node sees it -- web's `spec/architecture/console.md`.

## Decision

- **The hook keeps every `workflow_run` event**, queued and in progress as well as completed, with
  the commit message, branch and actor.
