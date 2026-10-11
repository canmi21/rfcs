---
name: "The Gateway's Cleanup"
title: "A pass over what the gateway's move left behind and held while it ran, the last of its milestones"
status: accepted
supersedes: []
blocked-by: [5c8hn]
commits: []
references: []
---

## Question

Every API moved behind one gateway, in milestones E1 to E12, each shipped on its own -- platform's
`spec/architecture/gateway.md`. What was found and held while the move ran is left; this is E13.

## Decision

A pass over what the move left behind:

- **host, which has no way to forget an app removed from the repository**: umami was forgotten by
  hand, its row deleted from host's store and host restarted so Caddy was rendered without it, its
  containers, network, image, data and snapshots removed over SSH.
- **The service domains' placeholder pages**, once the domains answer at their apexes.
- **The lifetimes the CDN and the alias layer still stamp themselves**, settled here.
