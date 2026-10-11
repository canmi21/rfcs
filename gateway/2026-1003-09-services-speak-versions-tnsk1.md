---
name: Services Speak Versions
title: 'Every service routes on its version, /v1/ born in the shape the addresses rule gives'
status: landed
supersedes: []
blocked-by: [3uc3y]
commits:
  [
    platform@f066a803,
    platform@c9c12938,
    platform@96ae7804,
    platform@1d81ca7e,
    platform@b1e73b16,
    platform@92a6e1f2,
    platform@cef3babd,
    platform@693d7880,
  ]
references: []
---

## Question

A service's paths had no version, so a change of shape could not be made beside the old one. E4 of
the gateway's milestones.

## Decision

- **Every service routes on `/v{n}/`**, the CDN on `/v3/` and the rest on `/v1/`.
- **`/v1/` is born in the shape the workspace's `spec/addresses.md` gives**: a route that put a
  thing's identity in its query is reshaped as its first version is written, rather than as a second
  version later, its service and every caller moving together.
- **Both spellings answer for as long as anything sends the old one.** A node's service and the
  gateway deploy apart, so the unversioned path answers as it did until the last caller moves, in
  E8.
