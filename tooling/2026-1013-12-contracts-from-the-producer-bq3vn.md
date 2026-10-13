---
name: 'Contracts From the Producer'
title: 'A contract read in another language is a JSON Schema its producer generates from its own type, checked for drift'
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The registry's catalog, `GET /v1/kinds`, is written in Rust and read by the command line in Python
and by the console in TypeScript. What the relay sends the console is already copied by hand from
its Rust types into the console's `wire.ts`. On 2026-10-13 the command line and the registry each
guessed how the catalog would describe a list of objects and a write-only field, and agreed only
because the lead told both. The author asked for a contract layer that keeps the readers from
drifting: a library within one language, something like JSON across languages.

## Decision

Decided with the author on 2026-10-13; the rule is the workspace's `spec/json.md`, "A contract across
languages is the producer's type, and every other reader is derived from it".

- **The producer's type is the one source.** A Rust service derives `schemars::JsonSchema` on the
  types it answers with and writes their JSON Schema to a committed file beside the crate.
- **A check generates it again and fails on a difference**, so a type changed without its schema
  fails where it was changed, not in a reader later.
- **Readers in another language are derived**: the console's TypeScript types are generated from
  the schema, and a Python reader's tests validate their fixtures against it. Nothing is copied by
  hand.
- **The order**: the registry's catalog first, `/v1/kinds` and a resource's envelope; then what the
  relay sends the console, replacing the hand-copied `wire.ts`.
