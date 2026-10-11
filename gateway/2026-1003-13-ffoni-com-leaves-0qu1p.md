---
name: ffoni.com Leaves
title: "ffoni.com's profiles, route and rules are deleted once nothing calls it, rdm's builds the last"
status: landed
supersedes: []
blocked-by: [v987q]
commits: [platform@714e78c8]
references: []
---

## Question

`ffoni.com` was the old domain of the service layer. Once every caller had moved, what of it? E9 of
the gateway's milestones.

## Evidence

`cdn.ffoni.com` was bound back on 2026-10-08, the live corpus still naming it: RFC q3054.

## Decision

- **Its profiles, its route and its rules folder are deleted** once nothing calls it -- rdm's
  installed builds the last -- and nothing here routes to or calls the domain.
- **The zone and the registration are the owner's to drop.**
