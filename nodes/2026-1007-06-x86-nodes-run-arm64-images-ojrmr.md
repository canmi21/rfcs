---
name: x86 Nodes Run arm64 Images
title: 'An x86 node may run arm64 images by emulation, declared by the node and asked for by the app'
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Some images are built for arm64 alone, the platform's Postgres among them, and some nodes are x86 --
infra's `spec/architecture/nodes.md`, "An x86 node may run arm64 images, emulated, and never the
other way".

## Decision

- **`emulate = ["arm64"]` in `nodes.toml`**, for which `mise run node` installs QEMU's user-mode
  emulation.
- **`arch = "arm64"` in an app's `service.toml`**, for which host fetches that architecture's
  artifact and refuses to place it where it cannot run -- infra's `spec/architecture/host.md`, "An
  app may ask for one architecture".
- **`buf` first**, for the platform's Postgres.
