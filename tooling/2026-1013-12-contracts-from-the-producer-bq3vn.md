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
- **Readers in another language are derived**, nothing copied by hand. A reader's test fixture is
  the producer's own answer, committed and drift-checked as the schema is, since a schema holds the
  shape and only the answer holds the values.
- **A TypeScript reader is generated twice from the schema**, decided with the author on 2026-10-13:
  its types, and a valibot validator, valibot being what the TypeScript code already validates with.
  The generator is the workspace's own, over the part of JSON Schema the producers write -- objects,
  enumerations, arrays, optional keys, references -- and refuses the rest. `ts-rs`, `specta` and
  `typeshare` give types and no validation; Protocol Buffers fit binary RPC rather than JSON in an
  envelope; OpenAPI describes a whole HTTP API, more than a contract needs; a schema written in
  valibot and exported would put the source away from the producer that answers.
- **Both languages run one set of examples**: `valid/`, the producer's committed answers among them,
  and `invalid/` -- a key missing, a choice outside its list, an unknown key where the type forbids
  one. The producer's tests and every reader's run them all and must agree on each.
- **The order**: the registry's catalog first, `/v1/kinds` and a resource's envelope; then what the
  relay sends the console, replacing the hand-copied `wire.ts`.
