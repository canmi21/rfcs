---
name: canmi.app Goes to Space
title: 'canmi.app goes to space, its three Workers joining the deployer, the parts before the router'
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Space is built -- web's `spec/architecture/space.md` -- and `canmi.app` still redirects to the site.

## Decision

- **`canmi.app` goes to space**: platform's redirect rule in `rules/canmi.app/` comes off the apex,
  the sdk names `canmi.app` and its paths.
- **Space's three Workers join the deployer**: `space`, the router, and `space-design` and
  `space-rfcs`, the parts it binds, in the deployer's `WORKER_OWNERS` and
  `.mise/tasks/deployable`'s `MOVED`, the parts deployed before the router that binds them.
