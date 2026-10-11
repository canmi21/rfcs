---
name: One List of Ranges
title: "One list of the ranges, 1h to 30d, in place of the three the console's pages carry"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The console's pages carry three lists of ranges, and the live panel its own copy of run grouping.

## Decision

- **One list of the ranges**, `1h` to `30d`, in place of the three.
- **Run grouping moves out of `lib/server/`** so the live panel stops keeping its own copy.
