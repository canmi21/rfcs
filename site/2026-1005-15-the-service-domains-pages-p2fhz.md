---
name: "The Service Domains' Pages"
title: "The service domains' pages, web's half of what the landing pages decide"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `spec/architecture/landing.md` decides that each service domain answers a page of its own.
What is web's half of it?

## Decision

- **`symlink.si` leaves `apps/landing`** for a static site of its own on the CDN, proxied by
  Cloudflare, with the names laid over it as a Worker route. The host's side is the platform's.
- **`monoflake.com` gets a site of its own**, and `monoflake.net` sends to it.
- **The records that send `ixc.one` and `il.lli.lil.ill.li` to `apps/landing`.**
