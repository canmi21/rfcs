---
name: The Gateway Enforces
title: "The gateway enforces each route's declaration and derives every host's own files, policy.ts retired"
status: landed
supersedes: []
blocked-by: [57fc8, fwzvc]
commits: [platform@7db6894f, platform@ff6090a9, platform@6237bad7, platform@31e81012]
references: []
---

## Question

Once every route is declared and every hostname read as a profile, what the gateway does with a
request can be derived rather than written. E3 of the gateway's milestones.

## Decision

- **CORS, the four lifetimes and crawling per route**, from the table.
- **Each host's `robots.txt`, `security.txt`, `favicon.ico` and path rule**, derived; a host admits
  crawlers by its profile, the CDN and the alias hosts admitting them and the API hosts refusing
  them.
- **`policy.ts` retires.**
