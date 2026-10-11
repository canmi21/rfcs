---
name: Deployer Records Unseen
title: "The console reads no record the deployer keeps, so a Worker's deploy never reaches the page the operator watches"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The deployer records every Worker's deploy, a failed one included -- platform's
`spec/architecture/deployer.md`, "What it refuses" -- and only `GET /api/deploys` serves those rows.
The console builds Now, the latest failures and the events from host's per-node `/events` alone,
keyed by node and app, so a Worker's deploy, failed or not, never reaches the page the operator
watches. Found on 2026-10-09, when a run the deployer could not read showed nowhere.

## Options

What deciding it involves:

- **Where the rows go**: the console reads `/api/deploys` beside host's events, or the deployer's rows
  reach host's events or the relay, so the console keeps one source.
- **A placement with no node**: the console's `Placement` is a node's app; a Worker runs on none, and
  a run that failed before its Workers were known names no Worker either.
- **host has the same gap for images**: infra's `apps/deploy/host/src/rollout/run.rs` logs a run whose
  artifacts it could not fetch and records nothing.
