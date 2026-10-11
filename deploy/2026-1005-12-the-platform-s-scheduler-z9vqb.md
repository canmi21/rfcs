---
name: "The Platform's Scheduler"
title: "The platform's scheduler runs a job once across the platform under a lease in Postgres"
status: accepted
supersedes: []
blocked-by: [xsarw]
commits: []
references: []
---

## Question

Background work has two schedulers -- platform's `spec/architecture/scheduling.md`, "Background work
has two schedulers" -- and the platform's own is not built, so the deployer, the probe and telemetry
are each held to one node.

## Decision

- **A lease in the cluster runs a job once across the platform**: the deployer on the three cores, and
  the probe and telemetry off the one node each is on.
- **Second of the services that keep nothing**, after ledger is in the cluster.
