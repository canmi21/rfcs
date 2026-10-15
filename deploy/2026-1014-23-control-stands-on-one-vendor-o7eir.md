---
name: 'Control Stands on One Vendor'
title: "The platform's control runs as two Workers over one D1 database in a Cloudflare account of its own, on the free plan, and one node speaks to it for the cluster"
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

The first answer reached for every primitive the vendor sells -- Durable Objects, Queues, Workflows
-- and moved the secret store there too. The author asked what starting would cost on the free plan
and on the $5 one, whether those primitives were needed at all, and drew the line the decision
below keeps: a part of control depends on a third-party cloud, and what runs on the platform depends
on ourselves.

## Decision

Decided with the author on 2026-10-14.

- **The direction is five stages, each built on the one before.** Control is built on Cloudflare's
  serverless primitives and schedules our nodes; then one model, the cluster depending on that one
  vendor for control and our nodes the pool it schedules, what the nodes ran only to keep control
  alive going with it; then one binding layer, an app declaring a database, a bucket or a browser and
  the platform resolving it to the vendor's service or a node's; then the layers below replaced
  without the app seeing it -- encryption with our keys, compression, content addressing, hot moves
  between stores, backups as one more store; and only once the platform is stable and carries enough
  that one vendor is the risk, a second vendor for control and egress. A vendor's name stays at the
  binding edge until then, so a second vendor is one more adapter.
- **Control has a Cloudflare account of its own**, `monoflake`, made beside the account that holds
  the author's sites and the projects the platform hosts, so nothing a hosted project does reaches
  control or its quota. Its domains are `monoflake.net`, moved first -- its `api` and `cdn` twins dark
  meanwhile, nobody asking them while `monoflake.com` answers -- and `monoflake.com` once the
  platform's own hosts follow. `monoflake.net` is control's alone: the console at its apex, locked by Cloudflare Access, until
  a public page takes the apex and sends a signed-in visitor on to it; the control API at
  `control.monoflake.net`, outside Access, since a node proves itself by its key.
- **The account starts on the free plan, and a feature is used only once it is confirmed free and
  failing closed** -- an exceeded limit an error, never a charge: Workers, D1 and its read replicas,
  Cron Triggers, service bindings, static assets and Smart Placement all qualify. Moving to the $5
  plan, a request budget and its alarms are decided once control runs and carries load, not before.
- **Stage 1 is two Workers and one database, nothing else.** `control` is the API that controls the
  platform itself -- distinct from the gateway, the API the platform serves -- and the only binding to
  the database; `console` serves the console and reaches `control` by a service binding. The database
  is D1, `control`, placed in western North America (`wnam`), the middle of the nodes it answers, with
  Smart Placement keeping `control` near it. Durable Objects, Queues and Workflows are not used: a
  revision is a conditional `UPDATE`, a timer is a cron, and a step runs on our nodes through the
  executor, never on the vendor's. R2 waits until something needs it.
- **Only control's own secrets are on Cloudflare**, as Worker secrets, the vendor trusted with them.
  A secret of what runs on the platform -- an app's values, the keys that encrypt a store -- never
  reaches the vendor, which has no use for it: the secret service on our nodes keeps them, and tier 1
  moving out of `secrets.json` into it, as RFC 231yq decides, goes ahead.
- **A node is its id, and the id is its key's.** Each node makes an Ed25519 key pair and keeps the
  private half; its id is the first 64 bits of the public key's SHA-256, sixteen hex digits, so a
  node proves its id by its key and is added or removed as one row. Beside the id a node has a
  `city`, capitalized and free to repeat -- two nodes may both be `Tokyo` -- a `country` and a
  `continent` as two-letter codes, and the coordinates the registry already keeps. The three-letter
  names retire: the database never has them, and the nodes drop them in a step of their own.
- **One node speaks to control for the cluster, every five minutes.** It is the live node with the
  lowest id in relay's mesh, the next taking over when it goes; two speaking at once is harmless. Its
  beat carries the mesh's view of every node, so control knows each node, not only the speaker, and
  the answer carries the registry's revision, which the speaker pulls when it differs and spreads
  over the mesh. Beats between nodes, telemetry and liveness stay in the mesh and never reach the
  vendor.
- **Control acts when the beats stop.** A cron every five minutes marks the cluster lost after three
  beats missed, then calls the other nodes in turn, from the last beat's addresses, to take over
  speaking; when none answers, it tells the author.
- **A change made in the console is one call, whole without the browser.** The browser calls
  `console`'s own API behind Cloudflare Access, whose token the Worker verifies again; `console`
  calls `control`, which writes the change to D1 by a conditional `UPDATE` on the revision, pushes it
  to the speaking node signed with control's key, and answers. The write is the change; the push
  only hastens it, and a push lost is caught by the next beat's revision. Access signs in by GitHub,
  the author alone admitted, once its free plan is confirmed as above. A console reading live data
  reads it from our nodes, not from control.
- **The move is one part at a time, the old part retired only after the new one carries the load**:
  the registry first, host reading both and acting on the new one only once they agree, as the
  secret service's reads did, and the cores' registry retired after; then the console; the ledger's
  place is decided after that. RFC bz8nz's measure stands: the count of acts done by hand.
