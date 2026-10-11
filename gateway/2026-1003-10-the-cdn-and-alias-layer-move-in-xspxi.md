---
name: The CDN and Alias Layer Move In
title: 'The CDN and the alias layer become services behind the gateway by binding, their own hosts given up'
status: landed
supersedes: []
blocked-by: [3uc3y, tnsk1]
commits: [platform@13d2fb11, platform@bbab78b0, platform@88040f39, platform@dc802c83]
references: []
---

## Question

The CDN and the alias layer were Workers of their own, each with its CORS, stamps, host files and
custom domains. E5 of the gateway's milestones.

## Evidence

What the two still stamp themselves is RFC 15kjm.

## Decision

- **Both become services behind the gateway, reached by binding**, their own CORS, stamps, host
  files and custom domains gone.
- **`ill.li` and the old hosts become profiles.**
- **A Worker behind the gateway asks another by binding, never through the gateway's host**: the
  alias layer asks the site by binding, and the gateway resolves its own marks through the alias
  layer's -- a request in a circle is one Cloudflare refuses.
