---
name: 'Two Scopes, One Name'
title: 'Two scopes may hold one app name, every container, network, data directory and route keyed by both'
status: accepted
supersedes: []
blocked-by: [nzscl, q6lab]
commits: []
references: []
---

## Question

A deploy belongs to a scope, decided on 2026-10-08 so a second owner's repository can be a source
without reaching another's apps -- platform's `spec/architecture/scheduling.md`, "A scope is an
organization, and only the boundary isolates". Each source carries its scope, and each app the scope
that first deployed it, since 2026-10-08 -- infra's `spec/architecture/host.md`, "The machine pulls;
nothing pushes into it". An app's name is still one across scopes.

## Decision

- **Two scopes may hold one name**, every container, network, data directory and route keyed by
  scope and name.
- **It waits on accounts**: there is no second owner to need it before them.
