---
name: 'Traffic and Disk Budgets'
title: "How a node's monthly traffic and its disk are measured, split between the system and the apps, and held to a limit"
status: draft
supersedes: []
blocked-by: [32hd9]
commits: []
references: []
---

## Question

The author asked on 2026-10-13 how a node's own resources are limited: measure more of its disk
and its network, set hard limits, and hold a monthly traffic allowance -- 10 TB at Oracle, 200 GB
at the smallest held today, and 1 GB if a free Google Cloud machine is added. The allowance is split
between the apps and a reserve the system keeps, by a function that holds from 1 GB to 10 TB: at
10 TB a percentage left to the system is more than it needs, and at 1 GB every megabyte counts.
Past a limit the gateway refuses with a code of its own; later a webhook, or the provider's API
cutting the machine off. Soft limits come first.

**What is measured today** -- infra's `spec/architecture/meter.md`: every second, the bytes each way
on the interfaces that leave the machine (a tunnel's bytes, `tailscale` among them, are already on
the physical interface, so tunnels are left out rather than counted twice), each container's own,
and each disk's reads and writes, kept a year at the coarsest grain. Not measured: a month's total
each way, the system's share against the apps', and the space each app takes on disk.

## Options

**The leaning, not decided:**

- **The system's need is a floor, not a share.** What the system sends -- the relays' snapshots to
  eight peers every three seconds, tailscale, image pulls, packages, the database's replication --
  follows the node's roles, not its allowance: an estimate of ten-odd gigabytes a month for the
  mesh alone, to be measured before it is trusted. So a node's reserve is the larger of its measured
  system traffic with a margin and a share that falls as the allowance grows:

  ```
  Q    the month's allowance, each way it is counted
  B    the system's traffic, measured, as a month
  p(Q) the share kept, falling with log10(Q) from 30% at 1 GB to 1% at 10 TB, the ends the author
       set on 2026-10-13
  R    = min(Q, max(1.5 B, p(Q) Q))      the reserve, 1.5 the author's margin until measured
  Q-R  the apps', paced by the day: what is left over the days left
  ```

  At 10 TB that keeps 100 GB, more than the system needs; at 200 GB about 13%, or the measure if
  larger; at 1 GB the measure decides, and a full member of the mesh does not fit at all.

- **An allowance is often the account's, and counts one way.** Oracle's 10 TB and Google's 1 GB are
  each the account's, shared by its machines, and count egress alone. So an allowance belongs to an
  `account`, RFC 32hd9, with the direction it counts and the day it resets, and the platform shares
  it among the account's nodes. **A node may hold an allowance of its own as well**, decided with
  the author on 2026-10-13, for a machine billed by itself: where both apply, the tighter binds.
- **Measured from what exists**: the physical interface's total each way, the system's containers
  and the apps' each added up, and the machine's own -- tailscale's among it -- as what is left; a
  month's running total each way; and each app's space on disk from btrfs's quota groups, which RFC
  vh1bl's stores already use.
- **Soft before hard.** At 50, 80, 95 and 100 percent of the apps' share, an event in the ledger;
  the gateway and the scheduler send an app's traffic to its other nodes, and nothing new is placed
  there. **Three codes, decided with the author on 2026-10-13**, each with its own code in the
  envelope: an app's share of traffic spent on a node is `509 Bandwidth Limit Exceeded`,
  `quota_exhausted` -- in no RFC, but what hosting has long answered, and a client that does not
  know it reads it as `500` (RFC 9110); a write refused for an app's disk being full is `507
Insufficient Storage`, `storage_exhausted`; and a dormant node, below, is `503` with
  `Retry-After`, `node_dormant`, the one of the three that is temporary by definition. The system's
  traffic goes on out of the reserve. `429 rate_limited` stays one caller's own rate, and `520` to
  `530` are Cloudflare's own and never used.
- **Then the edges.** Each threshold is also a webhook -- to the author through the chat bot, `cue`.
  Last, and not yet, a cut-off by the provider's API -- stopping the machine, or taking its public
  address -- when the reserve itself runs out, configured per account and run as a `quorum`
  workflow, RFC 23wgz, an action that asks for confirmation, RFC cpc2b. A hard cap by nftables quota
  on the apps' traffic alone, never the system's, sits between.
- **A node sleeps rather than stops**, decided with the author on 2026-10-13. A node on a small
  allowance runs low-power: its heartbeat at half the rate, its apps running. When the resource it
  is held to -- traffic each way, or disk -- is spent, it goes dormant: it keeps beating and answers
  the cores for its own state, and takes no request for an app. Another node answers for it: the
  gateway sends an app's request to a node of the app that is awake, and only where none is answers
  `503` with `Retry-After` at the reset, its code `node_dormant`, apart from `509 quota_exhausted`,
  an app's share spent on a node still awake. `502` and `504` say an upstream failed, and a dormant
  node has not.
- **Each way is counted apart, and an allowance says how its provider counts.** A node's traffic is
  always two totals, `sent` (its upload, egress) and `received` (its download, ingress), each kept
  as the next rule says. An allowance names how they are used against it, `counts`: `egress` or
  `ingress` alone, as Oracle and Google count egress; `sum`, the two added; or `max`, the larger of
  the two, as some providers of small machines bill. The console draws both ways, and what is used
  against the allowance as the provider counts it.
- **A limit is a field of a resource, so the console and the command line set it**, asked by the
  author on 2026-10-13: a node's and an account's allowance, a node's reserve, an app's share of
  disk, each written in the registry and drawn from its catalog, RFC cpc2b.
- **The period is each node's own.** An allowance resets every month on a day of its own -- the day,
  and the hour in UTC -- given on the account and overridden on a node, since two machines of one
  provider may be billed from different days. A month that has no such day resets on its last.
- **A running total outlives every restart, and counts nothing twice**, asked by the author on
  2026-10-13. Four rules hold it:
  - **Only the kernel's counter is read, never a sum of what was added.** Each interface's count
    since boot only grows, in 64 bits, so what a sample adds is the counter now less the counter
    last kept; a meter started again any number of times takes up from the counter it kept.
  - **What was last kept is keyed by the boot**, the kernel's `boot_id` with the interface. The same
    boot carries on from its counter; a new one starts from zero, and what passed between the last
    sample and the boot -- a second at most -- is lost and said so. A counter lower than the one
    kept within one boot, an interface made again, is read as a start from zero, never as less.
  - **The total and the counter it was taken at are written in one transaction**, so a crash leaves
    both or neither, and never a total that a later start would add to again.
  - **A node reports its period's total, never an increment**, and the platform keeps the newest per
    node and period, replacing it -- a write that can be replayed, RFC bhybp. A node that lost its
    disk starts from the larger of its own and the platform's. A sample is counted in the period its
    time falls in, so a period's edge is off by a second at most.
- **A node's disk is modeled in two parts.** What the apps run from -- their images and each
  container's writable layer -- and the data the platform keeps -- each app's directory, RFC vh1bl's
  stores, a database -- each a btrfs subvolume under a quota group with a limit; and a reserve the
  system keeps for images being pulled, logs and snapshots, never handed out. Whether a container's
  writable layer can be held to a limit depends on Docker's storage driver, and is to be found out.

**Measuring starts now**, the author said on 2026-10-13, ahead of the rest: the meter keeps each
hour's exact bytes each way, durable across restarts by the rules above, so a period starting on any
day and hour is the sum of its hours, and the system's share and each app's disk are measured too.

**Still open:** how a container's writable layer is held to a limit, which depends on Docker's
storage driver and is being found out. **A cut-off is internal for now**, the author said on
2026-10-13: the gateway and the node stop an app's traffic themselves, and no provider's API is
spoken to until one is needed.
