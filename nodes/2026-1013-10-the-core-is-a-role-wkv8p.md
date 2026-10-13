---
name: 'The Core Is a Role'
title: "A node's code is a label the platform proposes and keeps, and the core is a role in the registry, moved by a workflow"
status: accepted
supersedes: []
blocked-by: [23wgz, 2kuvi]
commits: []
references: []
---

## Question

Three nodes, `tyo`, `sin` and `rdu`, hold the control plane while running apps like every other --
infra's `spec/architecture/nodes.md`, "Three nodes are the core, named by the author". The author
asked on 2026-10-13 whether the core should be scheduled rather than named: whether those three need
to be those codes at all, and whether every node, a core among them, could be given its code by the
platform.

What makes a node a core is today `core = true` in infra's `nodes.toml`, read by the tasks that
write host's grants and the database's members. Moving the core to another node is a sequence done
by hand: a database and quorum member added, the seal's shares split again, the registry and the
secret service deployed, the old member taken out.

## Decision

Decided with the author on 2026-10-13.

- **A node is its id; its code is a label.** Everything that refers to a node holds its id, RFC
  2kuvi. Nothing in a code says what the node does: `tyo` is a place, not the core.
- **The platform proposes a code when a node is registered**, the IATA code of the airport or city
  nearest where the node is, the next of that place's codes if the first is held -- `tyo`, then
  `nrt`, then `hnd`. The author may write another. Once given, it stays: DNS, the tailnet and the
  edge's bindings name it, so changing it is an act of its own, RFC 2kuvi, never a side effect of
  scheduling. A retired node's code may be given again, since what referred to it holds its id.
- **The core is a role, held in the registry**, `roles: ["core"]` in a node's `spec`, in place of
  `core = true` in `nodes.toml`. Being a core and running apps are both true of a core; its control
  plane is first in memory, RFC fr1vj.
- **Which nodes are the core is still the author's to say**, as a `spec` is. The platform checks the
  rules a core keeps -- three, one per failure domain, each native arm64, each the author's own --
  and refuses a write that breaks one, `forbidden_field` with the rule named.
- **A change to the core is carried out by the platform**, as a `quorum` workflow, RFC 23wgz: the new
  member joins the database and the quorum and catches up; the seal's key is split again so the
  shares are held by the new three; the registry and the secret service are deployed on it; and only
  then is the old member taken out. One core moves at a time, so two of three agree at every step,
  as RFC bhybp's membership writes require.
- **The order**: the role moves into the registry once resources have ids and workflows run; until
  then `nodes.toml` stays the source and the move stays by hand.
