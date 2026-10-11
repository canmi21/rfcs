---
name: 'B1: Local and Its Two Halves'
title: 'The content service is renamed local, an HTTP shell beside the CLI with a TypeScript half owning the database'
status: landed
supersedes: []
blocked-by: []
commits: [web@1a8166e2, web@ff14f4b3]
references: []
---

## Question

The content service was called the CMS and was a CLI: an editor in a browser could not reach it, and
nothing owned the authored database. B1 of web's milestones, the decision that keeps the later ones
cheap.

## Decision

- **The content service is `local`**, its crate, command and state directory renamed with it.
- **It answers HTTP**: the Rust half owns bytes and derivation and forwards `/collection` to a
  TypeScript half, behind a socket with no address of its own, which owns the authored database.
- **The editor is a web client from its first day**, so going online later is a host change rather
  than a rewrite.
