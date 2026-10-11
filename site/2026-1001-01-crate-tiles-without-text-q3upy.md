---
name: Crate Tiles Without Text
title: "The crate chart's tiles are links with no text"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The `cargo` block (web's `libs/prose/src/blocks/cargo/cargo.svelte`) draws each
dependency as a tile, and every tile is an `<a>` to its crate's registry page whose name is only an `aria-label`;
the words drawn on a tile sit outside it. Measured on the live
`development/rust-cargo-cranelift-tuning`, 2026-10-01: 254 of the page's 301 links carry no text,
every one of them a tile. To anything that reads a link's text -- a crawler, a translator, an answer
engine -- they are links with no name, and to a keyboard each is a stop of its own. Open; the
approach is the author's to decide.
