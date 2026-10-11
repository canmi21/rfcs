---
name: The House Answers Its Names
title: "A resolver at home answers the gateway's names with the node, and the private suffix moves to internal.ixc.one"
status: landed
supersedes: []
blocked-by: [6fiei]
commits:
  [infra@2c5da9bf, infra@57cee557, infra@5c2aedee, infra@784548a8, infra@ca8234ff, infra@09680c72]
references: []
---

## Question

A gateway at home is of no use until the LAN asks it. E12 of the gateway's milestones, carried out
in infra.

## Decision

- **infra's `apps/network/resolver`, CoreDNS rendered by host**, answers the gateway's hostnames with
  the node and passes the rest down a chain that skips what fails.
- **DHCP and the tailnet's split DNS point at it.**
- **The private suffix moves from `canmi.icu` to `internal.ixc.one`**, whose certificate the zones'
  token can renew.
