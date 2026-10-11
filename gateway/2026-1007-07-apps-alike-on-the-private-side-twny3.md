---
name: Apps Alike on the Private Side
title: "A node's private side cannot tell one app from another, and how each app is given an identity is open"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

A container asking `api.internal.ixc.one` for a private scope placed on another node is sent on with
`INTERNAL_TOKEN` because it is a container on the node, nothing more -- infra's
`spec/architecture/host.md`, "Every node answers the private API, and sends on what is not its own".
While every app is the author's that is the whole check; once friends' apps share nodes -- the
workspace's `spec/architecture/ship-cloud.md` -- a tenant's container would reach the platform's
private scopes the same way.

Each app carrying an identity of its own, which the private side checks before it lends the
platform's, is the direction; how it is issued and what each identity may reach are undecided, and
wait on accounts.

## Evidence

Until then one gap is known: a LAN connection that reaches Caddy through Docker's userland proxy
arrives from a bridge gateway inside the app range and is admitted as a container would be --
narrowing `APP_SOURCES` to a fixed pool for app networks, or turning the userland proxy off, would
close it.
