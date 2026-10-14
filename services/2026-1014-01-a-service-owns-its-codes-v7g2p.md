---
name: 'A Service Owns Its Codes'
title: "lib's response package keeps the envelope and a few codes any API has, and each service declares its own codes in its contract"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`@canmi/response` and the `response` crate carry one closed catalog of codes, `codes.json`, and
`Code` is its union: a service that needs a new code waits for lib to add it, release both halves
by hand at one semver version, and every consumer to take the bump. On 2026-10-14 the ledger needed
`stale_lease`, `replay_diverged`, `run_exists_differently`, `result_too_large` and `no_such_run`,
and answered under the nearest catalog code with its own word first in the message until lib
caught up. The author asked why the envelope decides which codes a service may answer: the
program produces its codes and shares them with its consumers, so the legal codes are the
program's.

## Decision

- **lib keeps the envelope**: the two shapes, `success`, `failure` and `unwrap`, the fixtures both
  halves are tested against, and **a short set of codes any HTTP API answers whatever it does** --
  `invalid_body`, `invalid_method`, `invalid_size`, `invalid_token`, `no_such_route`,
  `rate_limited`, `service_unavailable`, `upstream_unavailable` -- each with its default message.
  A code is any `^[a-z][a-z0-9_]*$`; `failure` takes one with its message, and the shared set is
  only the ones lib can word without knowing the service.
- **A service declares its own codes in its contract**, a `codes.json` beside its schema, each code
  with its HTTP status and default message, in the shape lib's set is written in. Its Rust and its
  TypeScript are typed from that file, so a misspelled code is still an error where it is written.
- **A reader takes a producer's codes as it takes its schema**, generated from the file the
  producer commits -- the workspace's `spec/json.md` -- so the console knows the registry's codes
  as it knows its catalog, and no reader waits on lib.
- **A code means one thing within a service**; two services may each have an `invalid_name`, and
  what keeps the words consistent across them is review and the naming spec, not a central list.
- **The catalog's other codes move to the services that answer them**, found by where each is
  used, and lib's next major drops them, the old `Code` kept one minor as a deprecated alias so a
  consumer moves when it moves. The ledger's five codes are the first a service declares itself,
  and never enter lib.
- **The reading of a service's codes moves into lib with that major**: what parses a service's
  `codes.json`, answers `failure` and `failure_with` by its status, and finds the codes a source
  names, as the ledger's `src/codes.rs` does first, becomes the `response` crate's and its
  TypeScript half's, and every service with codes of its own takes it there; `failure` in
  TypeScript then takes any well-formed code, and lib's `spec/response/envelope.md` stops holding
  every code to its four families. Decided with the author on 2026-10-14.
