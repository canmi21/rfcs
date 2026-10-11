---
name: "The Gateway's Declaration"
title: 'Every service declares its routes in service.toml, and one table is resolved from them'
status: landed
supersedes: []
blocked-by: []
commits: [platform@ae4dd54f, platform@599b2973, platform@90544985]
references: []
---

## Question

Every API was to move behind one gateway Worker -- platform's `spec/architecture/gateway.md`. What
the gateway enforces had been rules written by hand, one per host, and nothing held them to what
each service serves. What is the gateway to read? This is E1 of the gateway's milestones, the first
of thirteen, each shipping on its own.

## Evidence

The declarations are every `service.toml`'s `[api]` today, and the table is the gateway's
`apps/edge/gateway/src/table.ts`.

## Decision

- **`[api.defaults]` and `[[api.routes]]` in each service's `service.toml`**: its CORS, its
  lifetimes and its exposure per route, held to a schema by `mise run scopes`.
- **One table resolved from them**, richer than the declarations, the service codes resolved in the
  URL library.
- **Nothing a caller sees changes.** The gateway still answers as before; this is the step that makes
  the rest a matter of reading the table rather than of writing rules.
