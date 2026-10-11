---
name: The Console Writes
title: "The console restarts, deploys and rolls back, by a path that lends a Worker no node's root token"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

One console runs the system -- web's `spec/roadmap.md`. It is deployed by the platform's deployer,
and reads; what does it do next?

## Decision

- **It writes**: restart, deploy and roll back, by a path that lends a Worker no node's root token.
