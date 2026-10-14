---
name: 'Artifacts Move to R2'
title: "A run's artifacts move from Backblaze B2 to Cloudflare R2, and host waits longer after each refusal"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

RFC q28g5 put every run's artifacts in an S3-compatible bucket and left the provider to the author,
who chose Backblaze B2's `build-artifact`. On 2026-10-14 at 14:48 UTC every node's host began to be
refused: `403 AccessDenied`, "download bandwidth or transaction (Class B) cap exceeded". The cap is
the author's own, set on the account so a runaway is stopped rather than billed. Nothing deployed
until it reset: the ledger, the new executor and two relay changes waited.

The day had rolled most of the cluster several times -- the database's six members, host on nine
nodes, the ledger twice -- and each node downloads each image it runs. B2 charges little to keep a
byte and much to send one: free egress is three times the month's average storage, and the
artifacts are kept to two runs, so that allowance is small, while every node reads every image. A
host refused also asked again every few minutes, each ask a transaction of its own.

The author asked whether B2 could stay with Cloudflare carrying the egress, which B2 sends to
Cloudflare free under the Bandwidth Alliance.

## Decision

Decided with the author on 2026-10-14.

- **The artifacts move to Cloudflare R2**, which charges nothing for egress, and gives ten million
  reads a month before it charges for them. They are few and small -- two runs of each, at most a
  few gigabytes -- so R2's higher price per stored byte costs almost nothing; they are read by
  every node on every deploy, which is what B2 bills for. R2 is reached by the same S3 API with
  region `auto`, so `artifact.ts`, `libs/deploy`'s bucket client and the deployer change by
  configuration alone: endpoint, region, bucket and keys. R2 keeps no versions, so a delete there
  frees the bytes at once, and the purge of versions B2 needed answers nothing there.
- **B2 behind Cloudflare was rejected.** The Bandwidth Alliance makes B2's egress to Cloudflare free
  but not its transactions, which may be the cap that was hit; a private bucket would need a Worker
  to sign every read, one more thing to fail between a node and its image; and Cloudflare's terms
  send large files to R2, not through its cache.
- **The database's backups stay in B2**, RFC ujnb5: written all the time and read only to restore,
  which is what B2 is cheap for.
- **A bucket's refusal is waited on longer each time.** host and keeper asking the bucket for a run
  and refused -- a 403, a 429 or a 5xx -- ask again after a delay that doubles from a minute to an
  hour, per run, and reset when an ask is answered, so a cap reached does not go on being spent by
  the asks that reached it. A new run's notice is still tried at once.
- **The egress proxies admit R2's host** in place of B2's, so the nodes without IPv4 keep reaching
  the bucket.
- **Runs already in B2 are not copied.** Once the nodes are told R2's endpoint they read nothing
  else, so a run uploaded to B2 and not yet deployed is built again by a new run; the B2 bucket is
  emptied by hand after the move.
