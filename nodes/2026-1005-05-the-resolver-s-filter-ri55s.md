---
name: "The Resolver's Filter"
title: "The resolver's chain is fed a filter, where host passes it an empty list today"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The resolver's filter step is rendered and tested -- infra's `spec/architecture/host.md` -- and
host passes it an empty list. What feeds it?

## Decision

- **host feeds the resolver's chain a filter of its own**, in place of the empty list it passes
  today.
