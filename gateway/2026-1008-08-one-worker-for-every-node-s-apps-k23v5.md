---
name: "One Worker for Every Node's Apps"
title: 'A Worker on the route *.canmi.app/* sends each label to the node running its app, from a table kept at runtime'
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

On 2026-10-08 `qq` on `sha` answered 404 at `qq.canmi.app`: `*.canmi.app` is one wildcard record to
`rdu`'s tunnel, so only `rdu`'s apps were public. How does every node's app become public?

## Decision

- **A Worker on the route `*.canmi.app/*`** -- a route, never a custom domain, which would put the
  host's name in the certificate logs; the edge's `*.canmi.app` wildcard certificate covers every
  first-level name -- looks up which node runs the app a label names and passes the request there over
  that node's Workers VPC binding, WebSockets included, behind Cloudflare Access as now.
- **The table of labels to nodes is kept at runtime, never in code**, fed by what the relay knows of
  every node; a label it does not know goes on to the origin, `rdu`'s tunnel, as today.
- **No DNS record is made per app.** A new node still means the Worker's bindings redeployed until
  one VPC Network binding reaches every node.
