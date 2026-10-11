---
name: A Deploy Belongs to a Scope
title: "Each deploy source carries its scope and its owner's token, and each app the scope that first deployed it"
status: landed
supersedes: []
blocked-by: []
commits: [infra@b071d7f9, platform@67b866d8]
references: []
---

## Question

A second owner's repository was to be a deploy source, its one app on `sha`, and nothing stopped one
source's run reaching another's apps.

## Evidence

Two scopes holding one name waits for accounts: RFC xfgvy. Where the sources and tokens are kept is
RFC eiazr.

## Decision

- **A scope is an organization, and only the boundary isolates** -- platform's
  `spec/architecture/scheduling.md`.
- **Each source carries its scope and its owner's token**, and each app the scope that first
  deployed it; a run reaches only its own scope's apps.
