---
name: 'Nodes and Edges'
title: 'Where a workload runs is a node or an edge; a node says how it is hosted, its account, its failure domain and its roles'
status: accepted
supersedes: []
blocked-by: [2kuvi, wkv8p]
commits: []
references: []
---

## Question

A node is described today by three declared axes in infra's `nodes.toml` -- infra's
`spec/architecture/nodes.md`, "Three things are declared, and the rest is measured": a `tier` of
`datacenter`, `home` or `transient`, a failure `domain` written as a provider's short code, and an
`expiry` year -- plus `core = true` on three of them. Cloudflare's Workers, where some apps run, are
no resource at all, named only by `glo` and `cf` inside a deployment's hostname. The author asked on
2026-10-13 how these become resources the registry holds: whether a Worker gets an id as a node
does, how a cloud machine, a machine at home and a serverless runtime are told apart, and whether
the tier says what a node is for.

The author's point on the last: which is better depends on the workload, not the node. A public
API is better on a provider's machine, since one lost is replaced and one expiring is renewed or
replaced as easily. Data for the author alone, or a few people, is often better at home, where the
space is larger and the data is in the author's own hands.

## Decision

Decided with the author on 2026-10-13.

- **A workload runs on one of two kinds, `node` or `edge`**, each a resource with an id, RFC 2kuvi,
  and an app's placement names either by its id. A kind is a whole word in code, never a short
  code.
  - **A `node` is a machine the platform runs host on**: it is measured by meter, meshed by the
    relay, and can hold the control plane.
  - **An `edge` is a serverless runtime someone else runs**, Cloudflare's Workers or Vercel: no
    machine and no host, its `spec` the provider, the account and the names of the credentials that
    reach it.
- **A node says four things of itself, each its own field:**
  - **`hosting`, `datacenter` or `selfhost`**: a provider's machine, or one in the author's own
    hands.
  - **`account`, the id of an `account` resource**, the paid account the machine is under, which
    a `selfhost` node has none of.
  - **`domain`, the id of a `domain` resource**, which nodes fail together, grouped by hand.
  - **`roles`, at least one, as many as it holds**: `compute`, which runs functions and services;
    `storage`, which keeps data, a store's share of its disk (RFC vh1bl) and an app's database; and
    `core`, which runs the platform's database and schedules, moved as RFC wkv8p says.
- **An `account` is a resource of its own**, changed with the author on 2026-10-13 from a provider's
  short code such as `oci-2`. One paid account is one account, so two at one provider are two, as
  `tyo`'s and `sin`'s Oracle accounts are. Its `spec` is its `provider`, a whole word -- `oracle`,
  `azure`, `racknerd`, `cloudflare` -- and `identities`, its name being the resource's own, optional and as
  many as it has: the provider's user id, its tenancy, the email it signs in with. An identity is
  kept in the secret service, RFC 231yq, and the account holds only its name there, as a store's
  credentials are named and never kept, RFC vh1bl; the registry is read by the whole tailnet. An
  `edge` names its account the same way.
- **A `domain` is a resource of its own: what fails together, as the author groups it.** An account
  and a domain answer different things -- an account ends with a card or a contract, a domain goes
  down with a provider's incident or a house's power -- and often match without being the same. Each
  node names its domain by hand; today each paid account is its domain, and each house its own. The
  core's rule of one per failure domain, RFC wkv8p, counts this field.
- **What a node is for is the workload's to say, not the node's.** A public service is placed on a
  `datacenter` node; data kept for the author, or a few, may be kept on a `selfhost` one. Today
  this is said by where an app is placed and which stores a bucket's layout names; when placement
  is chosen by the platform, a workload states it as a requirement or a preference against
  `hosting`, and the scheduler matches it.
- **`transient` and `expiry` leave the declaration.** How long a machine is held is its lifecycle,
  abstracted later on its own.
- **A deployment's hostname stays as it is**, `{service}-{region}-{provider}.ixc.one`, until the
  gateway binds to these resources, when it is reconsidered.
- **Whether data kept on a `selfhost` node also keeps an encrypted copy elsewhere** is not decided
  here.
