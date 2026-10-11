---
name: Shot Keeps Nothing
title: 'shot keeps nothing, its queue in the cluster, its pictures in a bucket, its browsers on any node'
status: accepted
supersedes: []
blocked-by: [pxp3l]
commits: []
references: []
---

## Question

shot keeps its queue and its pictures on its own node -- platform's `spec/architecture/shot.md`.

## Decision

- **Its queue in the cluster, its pictures in a bucket, its browsers on any node**: sixth of the
  services that keep nothing.
