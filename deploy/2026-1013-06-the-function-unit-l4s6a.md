---
name: 'The Function Unit'
title: 'A function is code run in an isolate per call, with fixed limits, its assets kept by content and bound to buckets and databases that outlive it'
status: accepted
supersedes: []
blocked-by: [23wgz]
commits: []
references:
  - url: https://developers.cloudflare.com/workers/platform/limits/
    took: '64 MiB of code, 128 MB of memory, 30 s of CPU by default and 5 min at most, 1 s to start, read on 2026-10-13'
  - url: https://github.com/cloudflare/workerd
    took: "Cloudflare's own Workers runtime, open under Apache 2.0, so the code a Worker runs runs on a node unchanged"
---

## Question

RFC 23wgz leaves open what runs a function, the short-lived and stateless unit a step or a request
is. A container takes hundreds of milliseconds to seconds to start and tens of megabytes to keep,
which is right for a service that stays up and wrong for one call. The author asked on 2026-10-13
what a function is and what it is given.

## Decision

Decided with the author on 2026-10-13.

- **A function runs in an isolate, one per call, never in a container.** workerd first, so a Worker
  written for the edge runs on a node unchanged; wasmtime with WASI later, for Rust and the rest.
  A service, long-running and of any language, stays a container.
- **Its code is at most 16 MB compressed for JavaScript, and 64 MB compressed for Wasm.** The
  measure is the compressed size, not a figure that flatters it.
- **Its memory is at most 128 MB an isolate, a ceiling and never a reservation.** A node does not set
  memory aside for the ceilings of what it runs, or nothing would run side by side; a node under
  load past its memory swaps rather than refusing.
- **Its CPU time is 30 s a call, and its wall time 1 min**, either raised up to 5 min by the
  function's declaration. Work longer than that is steps, RFC 23wgz.
- **Its global scope has 1 s to start.**
- **It has no scratch space.** What a call holds in between is in its memory.
- **Its assets are kept by content, in the store, for as long as a deployment of it is kept**: at
  most 64 MB a deployment. A deployment is a list of paths to content addresses; one rolled out
  keeps every address the last one shared with it, adds the new, and lets go of what no kept
  deployment names, so rolling back is choosing a list and nothing is copied. More than 64 MB is
  put in a bucket.
- **What outlives a deployment is bound to it, by the resource's id**: a bucket (RFC vh1bl) and a
  database (RFC 6jw04). A function's database is a service of its own that alone writes it, as D1
  is, since many short isolates cannot hold one SQLite file; a Postgres is reached through a pool.
