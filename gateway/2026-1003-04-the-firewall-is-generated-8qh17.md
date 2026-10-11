---
name: The Firewall Is Generated
title: "Each service-layer zone's whitelist is written from the gateway's table and synced by a task"
status: accepted
supersedes: []
blocked-by: [fwzvc, hab3a]
commits: []
references: []
---

## Question

The gateway moved behind one table of declarations -- platform's `spec/architecture/gateway.md` --
and the firewall's rules are still written by hand. This is E7 of the gateway's milestones, after the
hostnames read as profiles and the new domains answer, both done.

## Decision

- **Each service-layer zone's whitelist is written from the table**, inside the expression and
  rule-count limits, and synced by `mise run rules sync`.
