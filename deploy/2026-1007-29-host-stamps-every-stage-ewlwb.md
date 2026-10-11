---
name: Host Stamps Every Stage
title: "host stamps each stage of a deploy, not only the last reached, so a run's timeline has a bar per stage"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

A run's timeline in the console has one bar, the last stage reached -- web's
`spec/architecture/console.md`.

## Decision

- **Host stamps each stage of a deploy**, not only the last one reached.
