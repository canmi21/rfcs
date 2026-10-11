---
name: Pinned Images Reported
title: outdated reads the tag and digest every Dockerfile pins and says which have a newer one upstream
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`outdated` reports packages with a newer version, and moves none of them. The images every Dockerfile
pins -- the database's among them -- are not read.

## Decision

- **A newer pinned image is reported**, whenever there is room: `outdated` reads the tag and digest
  every Dockerfile pins and says which have a newer one upstream, the way it reports packages, and
  moves none of them.
