---
name: A Health-Check Stage
title: "A deploy's health check gets a stage of its own, once libs/deploy's replace_beside reports its progress"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Today `starting` covers a deploy's start and its health check together -- infra's
`spec/architecture/host.md`, "Every event is kept, and none is pruned".

## Decision

- **The health check gets a stage of its own**, which needs `libs/deploy`'s `replace_beside` to
  report its progress first.
