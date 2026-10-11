---
name: The House Has a Gateway
title: "The gateway and quota run again on the node at home, behind Caddy's inside side, for the LAN"
status: landed
supersedes: []
blocked-by: [rmfpp]
commits:
  [
    platform@4c2e539e,
    platform@3b28aff9,
    platform@14d312cd,
    platform@cc6fdbe2,
    platform@125e4f85,
    platform@f133847e,
  ]
references: []
---

## Question

With the private side retired for one entrance, a device at home reached every service through
Cloudflare and back. Could the house answer the gateway's names itself? E11 of the gateway's
milestones.

## Decision

- **The gateway and `quota` deployed again on the node, under Node**, reached through Caddy's
  `inside` side, and Caddy answering the gateway's hostnames on the LAN.
- **Services on Workers reached through the public gateway with `INTERNAL_TOKEN`**, a call counted
  once where it entered.
- **Checked by address against the public one.**
