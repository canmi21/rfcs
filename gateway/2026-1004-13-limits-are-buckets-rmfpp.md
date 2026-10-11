---
name: Limits Are Buckets
title: "Every limit is a bucket counted by quota, the gateway's own counter and the site's bindings retired"
status: landed
supersedes: []
blocked-by: [v987q]
commits:
  [platform@88723bc1, platform@94b24fce, platform@0a849b91, platform@93d6efdd, platform@a889d8ae]
references: []
---

## Question

The gateway kept a counter of its own, and the site counted with Cloudflare's rate limiting
bindings: two ways of counting, neither shared. E10 of the gateway's milestones.

## Evidence

platform's `spec/architecture/quota.md`.

## Decision

- **A limit is a bucket**, its arithmetic in `@monoflake/sdk/limits`, with `burst` in every row.
- **`quota` counts them**, on Workers with its Durable Object, keyed by service, row and subject.
- **The gateway's own counter and the site's rate limiting bindings retire** in favor of it.
