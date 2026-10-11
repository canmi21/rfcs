---
name: Console Reads From the Backend
title: "The console's server reads only what the backend holds, the fleet's readings and the primary among the last"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `spec/architecture/console.md`, "The console's server never waits on data; it only draws",
has the console's server read only what the platform's backend holds. Two reads still fan out across
the nodes from the Worker.

## Decision

Each becomes one read of the nearest relay:

- **The fleet's readings**: `fleetNow` and `fleetSeries` on Nodes.
- **The database's primary**: `primaryOf`, which asks each node's proxy in turn.
