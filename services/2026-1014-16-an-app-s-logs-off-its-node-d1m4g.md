---
name: "An App's Logs Off Its Node"
title: "An app's logs are kept and read only on its node, through host or SSH, and whether they are carried off it, where to and for how long is open"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

"Every line an app writes is kept": host writes a container's whole log to `/data/logs/<app>/`, one
file per version it ran, before the container is replaced, and both are read on the node -- infra's
`spec/architecture/host.md`. Clearing them out is "a later decision, made when the disk says so".
They are read through host's API by the SSH forward, or over SSH itself when host's API is what
broke. It is row O5 of platform's `spec/architecture/operations.md`, with no RFC.

Three things follow. A node that is gone takes its logs with it, so the logs of what failed are
lost with the failure. An app on several nodes -- the relay, `geo`, `cron` on every one, the ledger
on three cores -- is read node by node, and a request that crossed nodes cannot be followed. And the
disk the logs grow on is the node's `/data`, which the database's WAL and every app share.

What already leaves the node is the meter's minutes, carried by the mesh, and the telemetry service,
whose page is RFC nva8u. Readings stay on the node by design, RFC grmtb; whether logs should too is
the same question asked of a different thing.

The ways in sight, and what each costs:

- **Kept on the node, read from the console**: the console draws host's logs as RFC beos3 draws its
  inspect routes, each node's apart, and a retention host applies by size. The least to build, and
  nothing new to run. Against it: a node lost takes its logs; nothing is read across nodes.
- **Shipped to a store of the platform's**: host tails each container and sends batches to a bucket,
  RFC vh1bl, by app and day, compressed, kept for a fixed time; read by the console from there.
  Against it: buckets are not built; and searching them is a scan.
- **Shipped to a log service of the platform's**, indexed and searchable across nodes, a service like
  the telemetry one. Against it: one more stateful service, which must not log into itself.
- **Shipped to a provider**: Cloudflare's or another's log product. Against it: a vendor holds every
  line, secrets that leaked into one among them, and RFC bz8nz's platform would read its own state
  off somebody else's.
- **How long**: a few days on the node and longer off it, or the same everywhere, and whether a line
  is ever kept as long as a backup.

What would decide between them: how often a log has been needed from a node that was gone; whether
reading one app across nodes is wanted or a node at a time suffices; how large the logs grow per
day, which is unmeasured; and whether a line may ever leave the author's machines.
