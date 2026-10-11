---
name: A Node Added in Seven Places
title: 'A node is added by hand wherever the nodes are listed, and how it joins while everything runs is open'
status: draft
supersedes: []
blocked-by: []
commits: []
references:
  - url: https://developers.cloudflare.com/workers-vpc/configuration/vpc-networks/
    took: 'a VPC Network binding, which reaches any address a tunnel routes, named at runtime; in beta as of 2026-10-08'
---

## Question

Bringing `sha` up on 2026-10-08 took an edit wherever the nodes are listed, and three of those were
found only when something failed: every everywhere-placed app's `placements`, in infra and the
platform; the `hook`'s and the `gateway`'s VPC bindings and geo's nodes; web's console, its bindings,
its map places and its facts; the deployer's `WORKER_RESOURCES`, set by hand on `tyo`, whose refusal
kept the old console up counting eight nodes of seven; and every relay's `RELAY_PEERS`, which a relay
reads only when it is deployed, so `rdu`'s did not know `sha` until it was redeployed by hand. A node
should join while everything runs: listed once, and found by everything that needs it.

It was blocked on having somewhere to keep that list at runtime, and the cluster is that place now --
platform's `spec/architecture/databases.md`.

## Options

What deciding it involves:

- **The registry**: the nodes and their facts in the cluster, written once when a node is added, with
  `nodes.toml` either its source or replaced by it.
- **Placement by declaration**, already decided and waiting as its own RFC.
- **Relays learning peers at runtime**, from the registry or from each other, instead of a file read
  at start.
- **The console reading the node list** from the relay's state, the facts and places with it, instead
  of a copy in its source.
- **The VPC bindings**: a VPC Service binding reaches one host and port fixed when the Worker is
  deployed, so a new node means a redeploy of each Worker that reaches every node. A VPC Network
  binding to the account's Cloudflare Mesh, `network_id = "cf1:network"`, reaches whatever address
  any tunnel routes, named at runtime by the URL fetched, so each node's tunnel routing an address of
  its own would let one binding reach every node, a new one included, with the deployer admitting that
  one binding rather than a list -- in beta, free while it is, as of 2026-10-08.
