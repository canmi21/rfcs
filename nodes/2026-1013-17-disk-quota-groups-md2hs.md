---
name: 'Disk Quota Groups'
title: "Each app's space on a node is counted by a btrfs quota group of its own, then held to a limit in stages"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

RFC j1ery measures a node's disk and holds it to a limit; this is its half for the apps' data. Each
app's directory is its own btrfs subvolume, and a deploy keeps three snapshots of it -- infra's
`spec/architecture/host.md`, "One version runs, and a failed deploy puts the last one back". What an
app takes is read from btrfs's quota groups, and on 2026-10-13 no node had them on, so the meter
answered no app's space.

btrfs has two kinds. **Full quota groups** follow every extent to every subvolume that refers to it,
so an extent shared with a snapshot is counted as shared; the cost is accounting on every write,
heavy where snapshots number in the hundreds. **Simple quotas**, since Linux 6.7, charge an extent to
the subvolume that first wrote it and nothing else, which is cheap and has two consequences here:
an extent written before they are switched on is charged to nobody, and one written before a restore
stays charged to the subvolume the restore deleted, since host restores by making a new subvolume
from the snapshot.

On 2026-10-13 every node ran Linux 6.12 or later, held at most 6 GB under `/data`, and kept about
thirty snapshots.

## Decision

Decided with the author on 2026-10-13.

- **Full quota groups, not simple ones.** Simple quotas would count the data already written as
  nobody's and lose an app's history at every restore; the cost of full ones is in snapshot counts
  this platform does not reach.
- **An app is one group at level 1, holding its directory and every snapshot of it.** The group's
  `exclusive` is what removing the app would free, snapshots included, which is what it costs; its
  subvolume's `referenced` is what the app itself sees. Both are answered.
- **host manages them**, with its own ioctls, as it manages subvolumes; a snapshot and a restore
  join the app's group in the call that creates them, so a deploy needs no rescan.
- **Each is switched and set while running, from the API and the console.** Turning quotas on or
  off and setting or changing a limit take effect at once, with no app restarted and nothing
  remounted.
- **Three stages, each begun once the one before has run steady:**
  1. **Counting.** Quotas on, each app's group made and read, no limit. The switch is per node, on
     host's API, off until turned on, and turned on a node at a time: `nrt` and `hnd` first, the
     cores last.
  2. **Soft limits.** A limit per app, and an event at 50, 80, 95 and 100 percent of it, RFC j1ery;
     nothing refuses a write.
  3. **Hard limits.** The same limit given to the kernel as the group's `max_exclusive`, so a write
     past it fails with `EDQUOT`, and the platform answers `507 storage_exhausted`, RFC j1ery.
- **A limit is written in the registry, not on host**, as RFC j1ery has every limit: a field of the
  resource, edited from the console and the command line alike, RFC cpc2b. The console holds only
  host's read token, since the other is root on the machine, so its writes reach a node through the
  registry, which host reads every minute. Applying a hard limit is the controller acting, and is
  shadowed first, the platform's RFC jln34.
- **Docker's own data is outside**: `/var/lib/docker` is on the root filesystem, not btrfs, and what
  grows there is the containers' logs, RFC j1ery.
