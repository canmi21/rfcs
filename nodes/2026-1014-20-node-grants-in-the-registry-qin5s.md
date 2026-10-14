---
name: 'Node Grants in the Registry'
title: "Which roles a node grants its apps is written in git and the node task, and whether the registry holds it, as a node's own grants or read off its roles, is open"
status: draft
supersedes: []
blocked-by: [qcyb3, wkv8p]
commits: []
references: []
---

## Question

An app is given a role only where it asks for one and its node grants it, never by its name -- since
2026-10-04. host reads what its node grants from `GRANTS` in its `.env`, which `mise run node`
writes: each family's grants, then on a core node `CORE_GRANTS` from `infra/.mise/tasks/host_env.py`
-- `database:peer`, `quorum:peer`, `registry:peer`, `secrets:peer`, `secrets:seal` and more -- then
the node's own `grants = ["app:role", …]` in `nodes.toml`, each pair once -- infra's
`spec/architecture/nodes.md`, "A node may name grants of its own". It is how `sha`, outside the
core, runs a database standby. A change is a commit and `mise run node --all`, row C4 of platform's
`spec/architecture/operations.md`, with no RFC: RFC ja5hm moves what a repository is granted, not
what a node grants.

The registry has begun to hold what grants follow from. A node's `roles` -- `compute`, `storage`,
`core` -- are in its record since 2026-10-13, derived by the import from `nodes.toml`'s tier and
`core`, and RFC wkv8p makes the core a role in the registry, moved by a workflow. Moving the core
today means editing `core = true`, rewriting every host's `.env`, and then the hand moves of the
database, the quorum, the registry, the secret service and the ledger, as `sin` was made a core on
2026-10-12. Once the core is a role the registry holds, `CORE_GRANTS` written by a laptop task is
the one piece of it left behind.

The ways in sight, and what each costs:

- **Grants read off roles alone**: the registry, or host from the registry, maps each role to the
  grants it carries -- `core` to what `CORE_GRANTS` lists, a role per family -- and a node's own
  grants retire. `sha`'s standby would need a role of its own, a database member that cannot lead.
  Against it: a role invented for every exception.
- **A node's own grants as a field of its `spec`**, beside its roles, written from the console;
  host reads both from the registry, RFC jln34, and keeps the last it read. Against it: two sources
  of one answer on every node, the roles' and the node's own, to be merged as the task merges them.
- **Grants on the placement**: placing an app on a node with the role it asks for grants it there,
  the registry refusing a placement the node's roles do not allow. Against it: a grant stops being a
  fact of the node, so what a node lets run is read across every placement on it.
- **Left in the task as bootstrap**: grants change only with the core or a new privileged app, and a
  role given wrongly is root on the node, which argues for a commit and review. Against it: the core
  moving by workflow, RFC wkv8p, would still stop at a laptop task.

What would decide between them: whether a grant is a fact of the node or of what is placed on it;
how many exceptions like `sha`'s there are and are likely to be; and whether a role on a node, which
can be root on it, may be written from the console at all, or only after RFC 8vy7x signs what a node
is told.
