---
name: Egress Proxies
title: "Egress proxies, and EGRESS_PROXIES in libs/deploy's GitHub client, come before gvx and bru lose IPv4"
status: landed
supersedes: []
blocked-by: []
commits: [infra@b3bf1a63, infra@39349034]
references: []
---

## Question

`gvx` and `bru` are to lose IPv4 -- infra's `spec/architecture/nodes.md`, "The nodes" -- and a node
without it cannot reach GitHub.

## Decision

- **Egress proxies are set up, and `libs/deploy`'s GitHub client reads them from
  `EGRESS_PROXIES`**, before either node loses IPv4.
