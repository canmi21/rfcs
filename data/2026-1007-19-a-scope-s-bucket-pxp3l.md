---
name: "A Scope's Bucket"
title: "A scope's bucket, first version, its index in the cluster and its bytes laid out by pool and mirror"
status: accepted
supersedes: []
blocked-by: [cg93k]
commits: []
references: []
---

## Question

An app declares a bucket and the platform places it -- platform's `spec/architecture/scheduling.md`.
What is its first version?

## Decision

- **Its index in the cluster**; put, get, delete and list; laid out by `pool` and `mirror`.
- **References, the sweep and the scrub**, on the platform's scheduler.
- Fifth of the services that keep nothing.
