---
name: A Second Probe Place
title: "A second probe would overwrite the first's checks, since a check's row is keyed by its id alone"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

A check's row is keyed by its id alone, so a probe on the VPS declaring the same checks would
overwrite the node's -- platform's `spec/architecture/probe.md`. The key gains `place` when the
second probe is written; what else a second place changes is open.
