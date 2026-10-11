---
name: Catch-Up Stalls After Elsewhere
title: 'Catch-up seems to stall after a run placed on another node, unconfirmed, and what bounds a pass is open'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

buf's host took no run's notice for 25 hours on 2026-10-08 and 09: its log has no catch-up line
between 06:46Z, when it took a run of the author's private `cue` by `from_run` and wrote no row, and
its restart at 08:00Z the next day, when it caught up thirteen runs in seven seconds and again wrote
none for the one it took by `from_run`.

`every` awaits each pass in turn and `take` awaits `from_run`, in
`apps/deploy/host/src/rollout/catch_up.rs`; the reading is that `from_run` does not return for a run
whose app is placed on another node, and catch-up stalls until host restarts. Inferred from the log,
not reproduced. Open: confirm it, and whether a pass gets a deadline, a take that writes no row is
an error, or both.
