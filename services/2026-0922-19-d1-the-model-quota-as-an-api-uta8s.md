---
name: 'D1: The Model Quota as an API'
title: A machine that is always on wraps the local model CLI as an OpenAI-shaped API behind a tunnel
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The plan's milestones are agreed in order, each saying what it changes and what it cannot start before -- web's `spec/architecture/collection.md` holds the model they assume.

## Evidence

D1 depends on nothing and may be taken at any point. It is filed as long-term because nothing is
blocked on it today, and that changes the moment B3 through B7 make publishing cheap enough that
the volume goes up.

The machine D1 and D2 assume is made deployable first, so that what moves there arrives by a push
rather than by hand. See infra's `spec/architecture/host.md`.

## Decision

- **A machine that is always on, wrapping the local CLI as an OpenAI-shaped API behind a tunnel.**
