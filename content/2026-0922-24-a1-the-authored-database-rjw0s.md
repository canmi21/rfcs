---
name: 'A1: The Authored Database'
title: 'Resources, contents, the two metadata layers and the reference table, under one Drizzle schema'
status: landed
supersedes: []
blocked-by: []
commits: [web@f4aeab88, web@8b4aa2a9]
references: []
---

## Question

What the author writes was spread across text files and records. A1 of web's milestones, the first
with nothing before it.

## Decision

- **One authored database, under one Drizzle schema**: resources, contents, the two metadata layers
  and the reference table, and its first migration.
- **The collection's databases migrate from one mise task**, the constraints tested.
- **Authored and reader data are never rows in the same database**, the tooling shared and the
  databases not -- web's `spec/architecture/collection.md`.
