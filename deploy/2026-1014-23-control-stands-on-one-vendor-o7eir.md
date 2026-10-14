---
name: 'Control Stands on One Vendor'
title: "The platform's control runs on Cloudflare's serverless primitives and schedules our own nodes, which build the abstractions above it; a second vendor waits until the platform is stable"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

By 2026-10-14 the platform's control ran on three core nodes of its own: the registry, the ledger,
the secret service, the database and its quorum, each on Postgres under Patroni. RFC bz8nz had just
counted what was still done by hand, and the largest share of it came from that arrangement rather
than from what the platform offers: the five stateful apps rolled a core at a time by hand, the most
frequent hand act of the week; a ledger whose rolling restarts the database it stands on (draft
d8orh); two cores lost needing a person in a shell (draft f4czw); a secret service sealed and
unsealed. A by-hand deploy of the registry that day skipped a whole run on three nodes. The author
observed that bootstrapping the platform from a bare VPS upward is hard, and that the platform's aim
was never to replace serverless but to do what serverless makes costly or awkward -- many object
stores, storage encrypted at rest, a browser to call without paying per use -- and asked whether the
platform could stand on a vendor's shoulders for control, as Railway runs its API on a cloud and its
work on its own machines.

## Decision

Decided with the author on 2026-10-14, in five stages, each built on the one before.

- **Stage 1: control is built on Cloudflare's serverless primitives.** The platform's records and
  acts -- the registry, the ledger and the scheduler, the secret store, the deployer, and one door
  in front of them -- run as Workers over D1, Durable Objects and their alarms, Queues and Workflows,
  using those primitives rather than rebuilding them; the bill is the control's alone and small. The
  nodes keep what they last read and run without it, as host does today: control lost stops change,
  never service. A secret is stored sealed, its key held by the nodes, so the vendor holds only
  ciphertext.
- **Stage 2: one model.** The cluster depends on that one vendor for control, and our nodes are the
  pool it schedules: host converges a node on what control wants, and an executor on a node takes
  the steps control hands out. What the nodes ran only to keep control alive -- the control
  databases' high availability, their quorum, the manual rollouts of the apps control stands on --
  goes with it.
- **Stage 3: one binding layer.** An app declares what it binds -- a database, a bucket, a queue, a
  browser, a function -- and the platform resolves each to the vendor's native service or to one of
  our nodes', so a Worker and a node service, ours or a hosted project's, deploy side by side in one
  layer. A hosted project's Workers live in a Cloudflare account other than the one control and the
  platform's own foundation live in, so nothing a hosted project does reaches the foundation.
- **Stage 4: the layers below are replaced without the app seeing it.** Object storage and an app's
  database gain what the vendor's lack or charge for: encryption before data leaves the app, keys
  held by us; compression; content addressing, so content is placed, spread over several stores,
  joined and moved -- hot -- without its address changing. Backups follow from it: a store is one
  more place content is held.
- **Stage 5: only once the platform is stable, and carries enough that one vendor is the risk, a
  second vendor** takes a share of control and of egress. Until then a vendor's name stays at the
  binding edge, as `spec/naming.md` requires, so a second vendor is one more adapter, never a
  rewrite.
- **The move is one part at a time, the old part retired only after the new one carries the
  load**: the registry first, smallest and already read by host with its last answer kept; then the
  secret store; then the ledger and the scheduler; then the deployer. Each step is deployable alone,
  host changing only where it reads.
- **What this revisits**: RFC xsarw's ledger in the cluster, bhybp's every core taking writes,
  wkv8p's core as a registry role and 231yq's custody of secrets are taken up again against stage 1,
  and the drafts d8orh and f4czw are likely made moot by stage 2; each is superseded, if it is, by
  the RFC that moves its part. RFC bz8nz's measure stands: the count of acts done by hand.
- **Moving the task writers onto the secret service is paused** until stage 1 says where a secret
  is kept; host's reading of the service stays, the place it reads from to change.
