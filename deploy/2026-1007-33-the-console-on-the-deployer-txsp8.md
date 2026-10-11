---
name: The Console on the Deployer
title: "The console moves to web and is deployed by the platform's deployer, the first Worker to be"
status: landed
supersedes: []
blocked-by: [6s0e5]
commits: [web@a831366f, platform@cc0a252c]
references: []
---

## Question

The console was built in the platform, and the per-node panel beside it. Where does it live, and who
deploys it, once the deployer exists?

## Evidence

web's `spec/architecture/console.md`.

## Decision

- **The console is web's**, a services app built there, the per-node panel retired.
- **It is packaged for the platform's deployer** and deployed by it, the first Worker to move off
  Cloudflare's Git integration.
