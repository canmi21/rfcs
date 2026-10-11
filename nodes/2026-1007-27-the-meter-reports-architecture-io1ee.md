---
name: The Meter Reports Architecture
title: "The meter reports a node's architecture in its machine info, where the console reads a kernel release"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The console reads a node's architecture from the kernel release, and Alpine's `-virt` kernels give
none -- web's `spec/architecture/console.md`.

## Decision

- **The meter reports a node's architecture** in its machine info.
