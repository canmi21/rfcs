---
name: 'F: The Layers'
title: 'The site, the platform, infra and the library under them are laid out as layers, then split into repositories'
status: landed
supersedes: []
blocked-by: []
commits:
  [
    web@fb01f5e5,
    web@d38cebb0,
    web@5d8ec3bd,
    web@c89fc4d4,
    web@b9d51cd3,
    web@a58399fe,
    web@12d244cb,
    web@715e6b4e,
    platform@46c5d8d7,
    infra@9c87ff0a,
  ]
references: []
---

## Question

Everything the author ran lived in one repository, `canmi21/lattice`, and nothing held a package to
the layer it belonged to.

## Evidence

The workspace's `spec/architecture/layers.md`.

## Decision

- **Three layers and the library under them** -- the site, the platform and infra -- laid out in the
  one repository as the repositories they would become, and a check that fails a package depending on
  a layer above its own.
- **Then split**: `canmi21/lattice` becomes `canmi21/web`, and infra and the platform continue from the
  same commit, each lifted to its own top; the library is taken from its registries.
