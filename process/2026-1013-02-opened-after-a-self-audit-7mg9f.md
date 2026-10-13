---
name: 'Opened After a Self-Audit'
title: "The platform's repositories stay private while it bootstraps, and open once accounts exist and a scan it schedules itself finds them clean"
status: accepted
supersedes: []
blocked-by: [nzscl, 8vy7x]
commits: []
references: []
---

## Question

`platform`, `infra` and `cue` were made private on 2026-10-13, and GitHub's code scanning stopped
there, since it needs Advanced Security on a private repository. When do they open again?

## Decision

Decided with the author on 2026-10-13.

- **They stay private while the platform bootstraps itself.** Code half way through a change is
  not fit to be shown. A bug that is public is a bug in production, and a platform that cannot yet
  hold itself up cannot afford one exploited before it is fixed.
- **They open once accounts exist (RFC nzscl) and what a node is told to run is signed (RFC
  8vy7x).** Before that, the platform schedules a security scan of its own repositories, as one of
  its own jobs, and every finding is fixed or answered until the code is fit to be shown.
- **Then they are public, for anyone to audit.** Nothing in them is a secret: secrets are kept
  apart, RFC 231yq.
- **No code scanning runs on them while they are private.** A workflow that fails on every push for
  want of a paid feature is not kept.
