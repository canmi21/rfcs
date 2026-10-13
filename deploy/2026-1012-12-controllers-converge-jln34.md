---
name: Controllers Converge
title: 'Every layer runs a controller that brings what is toward what the registry wants, taking authority node by node after running in the shadow'
status: accepted
supersedes: []
blocked-by: [ja5hm]
commits: []
references:
  - url: https://kubernetes.io/docs/concepts/architecture/controller/
    took: 'a loop that compares the wanted state with the current one and acts to close the difference'
  - url: https://kubernetes.io/releases/version-skew-policy/
    took: 'one layer upgraded at a time, in a fixed order, each tolerating an older neighbor'
  - url: https://builder.aws.com/content/3K9SiqiJ369TGkkGzqPM95BuYWI/static-stability-using-availability-zones
    took: 'static stability: what runs keeps running on what it last had while the machinery of change is impaired'
---

## Question

With what the platform holds of where in its registry -- RFC ja5hm -- how does the system come to
match it, and how does the platform change itself without breaking itself? Adding `sin` on
2026-10-12 changed five layers in one afternoon, every step run by hand.

## Decision

- **The registry holds what is wanted, and every layer runs a controller that brings what is toward
  it**: a relay its peers, host what its node runs and at which release, the deployer the Workers
  and their bindings. Adding a node becomes one write and the command that boots the machine.
- **A controller compares the whole of what is wanted with what is, every round**, and a notice --
  CI's, the hook's -- only asks it to compare now. A notice lost changes nothing.
- **What a controller last read is kept on its own disk and never expires.** The registry
  unreachable stops a change, never a node.
- **A controller reads the registry by its API, never by its code**, so infra reads the platform's
  contract without depending on its crates.
- **What an app runs where, and at which release, is in the registry**: placement first, imported
  once from the declarations, which keep only what an app cannot run without; then each release a
  run built, written by the hook, a rollback being the wanted release set back. An app rolled out by
  hand -- the database, the quorum -- is kept present and never moved to a new release by itself.
- **A new controller runs in the shadow first**: it reports the difference it would close and closes
  none, the console showing it. Once the difference is nothing it takes authority on the canary,
  then node by node, each node able to fall back to the old path. The old source is removed from git
  last.
- **A controller limits what it destroys**: one removal a round, never a wanted state that empties a
  node, and nothing removed but what is retired -- a node or an app leaves by a `retired` mark, never
  by a record gone missing.
- **One layer changes at a time, in a fixed order, each tolerating its older neighbor**; a record or
  a schema changes by expanding first and contracting once no reader needs the old form.
- **The wanted state is unsigned until RFC 8vy7x**, accepted because only the author's own nodes run
  it, behind Access and on the tailnet; its records are shaped so a signature can be added without a
  new form.
