---
name: Tunnels Configured Differently
title: "Two nodes' tunnels, made by one task, are configured differently, and which shape is right is open"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`rdu`'s tunnel routes `*.canmi.app` to Caddy with WARP routing on; `tyo`'s, written later by
`mise run tunnel`, is `ingress: [http_status:404]` with WARP routing off -- seen 2026-10-07. Workers
VPC reaches both anyway, so nothing is broken, but two nodes made by one task differ. Whether the
task should write rdu's shape everywhere, or rdu's is a leftover from before VPC to be trimmed, is
undecided.
