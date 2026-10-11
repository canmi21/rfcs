---
name: "A One-Shot Command's Reads"
title: How a one-shot command reads the collection
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

More than half the readers of the three record
files are Rust -- ten of `media.yaml`'s, six of `metadata.json`'s -- and `local` holds no line
that opens the collection's database, because B1 gave the authored database to the TypeScript
half so that its schema is declared once. Every one of those readers is a command that runs and
exits: `local alt`, `local image`, `local gc`. `local serve` is one subcommand beside them, not a
thing they can assume is up.

So there are four answers and none is free. A command spawns the other half and pays node's
startup every run. A command requires `serve` to be up, which changes what the command line
promises. Rust opens the file read-only, which is a second set of column names to drift. Or the
commands that read records move to the other half, away from the derivation they were written
for. Blocks the second half of A2, and C5 behind it.
