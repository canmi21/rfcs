---
name: Host Records the Repository
title: 'host records the repository an app was built from, on the app and on each deploy event'
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The console keeps a list of which app belongs to which scope beside infra's and the platform's apps
-- web's `spec/architecture/console.md`.

## Decision

- **Host records the repository an app was built from**, on the app and on each deploy event, so the
  console's scope of an app is a fact it reads.
