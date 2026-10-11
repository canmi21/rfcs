---
name: Services Held to One Node
title: "Three services run on one node until the platform's scheduler exists, and which leaves first is open"
status: draft
supersedes: []
blocked-by: [z9vqb]
commits: []
references: []
---

## Question

Three services are on one node because a second copy would do the wrong thing, not because one is
enough. Each waits on the platform's scheduler -- platform's `spec/architecture/scheduling.md`,
"Background work has two schedulers" -- which is not built:

- **`deployer`, on `tyo`.** It belongs on all three cores -- infra's `spec/architecture/nodes.md`,
  "Three nodes are the core, named by the author" -- but the hook tells every placement, so three
  copies would each deploy every run, and each keeps its own record of the last run deployed. One
  deploys under the lease and the others wait -- platform's `spec/architecture/deployer.md`, "One
  deployer, on one node".
- **`probe`, on `rdu`.** Every copy would ask every check, multiplying what the rounds cost the
  gateway's free allowance, and each would keep its own history, so the gateway's `any` would answer
  a different one each time. Under the lease one asks and the rest stand by; or each node asks from
  where it is, which is the second probe's place.
- **`telemetry`, on `rdu`.** It reads its own node's meter and host, so a copy elsewhere describes
  another machine under the same scope. Reading every node's snapshot from the relay instead --
  platform's `spec/architecture/relay.md` -- would make every copy answer the same, and then any node
  can run it.

Which comes first, the lease or telemetry reading the relay, is undecided.
