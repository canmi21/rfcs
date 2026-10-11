---
name: Workers onto the Deployer
title: "The Workers move onto the platform's deployer one at a time, aka, cdn, quota and the gateway, the site last"
status: accepted
supersedes: []
blocked-by: [6s0e5, txsp8]
commits: []
references: []
---

## Question

The platform deploys a Worker as host deploys an image -- platform's `spec/architecture/deployer.md`.
The deployer runs, on `tyo`, as the hook's third receiver; web is admitted, its `deploy.yml` and its
webhook among the sources; and the console is deployed by it. What follows?

## Decision

- **The rest move one at a time**: `aka`, `cdn`, `quota`, the gateway, and the site last, each off
  Cloudflare's Git integration once it is deployed by the deployer.
