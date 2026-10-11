---
name: 'A6: Reference Counting'
title: 'The reference table decides reachability, and the sweep takes only what 24 hours of grace released'
status: landed
supersedes: []
blocked-by: []
commits: [web@6a9f71de]
references: []
---

## Question

Deleting must remove one thing and never what it pointed at, and something has to find what nothing
points at any more. A6 of web's milestones.

## Evidence

`libs/collection/src/sweep.ts` in web.

## Decision

- **The reference table decides reachability**, rebuilt rather than maintained.
- **24 hours of grace** before anything unreachable may go.
- **An explicit GC**, by hand here and scheduled in the cloud once there is one.
