---
name: "Where a Node's Facts Come From"
title: "Where the console learns a node's facts"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

A node's tier, failure domain, expiry year and system are infra's, in its `nodes/nodes.toml`, and
the console needs them for the node table and each node's header. Today
`apps/console/src/lib/nodes/facts.ts` copies them by hand, so a node added or retired in infra
leaves the console wrong until somebody edits both. Each node's host could serve its own row, the
relay could carry the table with the snapshots, or the build could read the file from infra; each
moves where the fact is held, and none is chosen.
