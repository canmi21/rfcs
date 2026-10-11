---
name: Every Page Reads Facets
title: "Each console page's load reads facets once its look is settled, the app's health under /api/ with it"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `spec/architecture/console.md`, "A component asks for its facet", covers the polished parts.
What of the rest?

## Decision

Each follows when it is polished, not before:

- **Each other page's load reads facets** once its look is settled -- Nodes, a node, Apps, an app,
  Deployments, a run -- the nodes lean wherever no events are drawn.
- **The app page's `health` moves under `/api/`** with that page, a facet like the rest.
