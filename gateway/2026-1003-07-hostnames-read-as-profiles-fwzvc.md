---
name: Hostnames Read as Profiles
title: 'A hostname is read from the right into a profile, and a request into its service, version and path'
status: landed
supersedes: []
blocked-by: [57fc8]
commits: [platform@621519a3]
references: []
---

## Question

The gateway answers many hostnames, old and new, and each needs the same reading: which service, at
which version, by which provider and region. Where does that reading live? E2 of the gateway's
milestones, after E1.

## Evidence

`apps/edge/gateway/src/profile.ts` and its tests.

## Decision

- **A profile table**, a hostname read from the right, with the provider and region registries.
- **Every request read into its tuple**: service, version and path, before anything is enforced.
