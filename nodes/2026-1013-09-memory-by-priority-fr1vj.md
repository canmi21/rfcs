---
name: 'Memory By Priority'
title: "A node's memory is shared by priority, the system's never swapped and the work it runs allowed to, and even the smallest node runs functions and keeps a store"
status: accepted
supersedes: []
blocked-by: [l4s6a, dmmk8]
commits: []
references: []
---

## Question

Functions take memory as a ceiling and never a reservation, and a sleeping service is frozen for the
node to swap out -- RFCs l4s6a and dmmk8. host today gives every container swap equal to its limit,
so none swaps past its ceiling -- infra's `spec/architecture/host.md`. Four nodes have about a
gigabyte of memory: `nrt`, `hnd`, `gvx` and `bru`. The author asked on 2026-10-13 whether the nodes
have memory enough to run functions, and whether the small ones should run any.

## Decision

Decided with the author on 2026-10-13. The figures are set when it is built.

- **Memory has priorities.** The system -- host, keeper, Caddy, the relay, the database, the quorum,
  the registry, the secret service -- is never swapped, and is the last to be pressed. What runs on
  it -- functions, and services that may sleep -- is allowed to swap, and is pressed first.
  Per container, through cgroup v2.
- **Every node's swap is raised** for what may swap: compressed memory, zram, first, since it is far
  faster than a disk and suits a node of a gigabyte, then a disk under it. `vm.swappiness` stays low,
  so what is active stays in memory and what is frozen or idle is what goes out. The sizes are
  measured before they are set.
- **Even a node of a gigabyte runs functions**, with fewer at once. A function keeps no state and its
  code is JavaScript or `wasm32`, so it runs alike on any node whatever its architecture or system,
  and a node too small for a database is still a place for it. Such a node also keeps a small store
  of its disk, RFC vh1bl.
- **The databases and what holds them run on the nodes with the memory**: `tyo` and `sin`, then `sha`
  and `rdu`.
- **One layer schedules them all**, by what each node is measured to have, never by its name or
  architecture.
