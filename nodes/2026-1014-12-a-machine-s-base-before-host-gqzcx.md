---
name: "A Machine's Base Before Host"
title: 'What a machine must be before the node task reaches it -- its system, Docker, Tailscale, a btrfs /data -- is made by hand, and whether an image, cloud-init or a workflow makes it, and restarts dockerd, is open'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`mise run node <name> --address` brings a machine to what `infra/nodes/` holds for it -- infra's
`spec/architecture/nodes.md`, "A node is set up by one task, run again at will" -- but it starts
from a machine that already has its system, Docker, Tailscale logged into the tailnet, and `/data`
formatted as btrfs and in fstab. Nothing makes that machine: `machine_setup.py` only renames it on
the tailnet and writes the units around a `/data` that exists. Three acts in platform's
`spec/architecture/operations.md` sit there with no RFC:

| Row | Act                                                                      | Today                         |
| --- | ------------------------------------------------------------------------ | ----------------------------- |
| N2  | Install Debian or Alpine, Docker and Tailscale; log in; make `/data`     | by hand over SSH or a console |
| N7  | Restart dockerd once the task has merged keys into `daemon.json`         | by hand; the task never does  |
| N19 | Fix a node's tailnet name, which one set in Tailscale's console outranks | Tailscale's dashboard         |

N7 is by design today: what dockerd reads only at start "is the operator's to restart, and the task
says so" -- nodes.md, "Docker's pool is its private range". `live-restore` keeps every container
running through the restart, so it stops no app. The pool exists because on 2026-10-13 `tyo`'s and
`rdu`'s newer networks fell in a range nothing admits and the registry on them could not reach the
ledger, and the fix took on each node only once somebody restarted dockerd there. N19 follows
from Tailscale's rule that a name set in its console outranks the one the machine asks for.

RFC bz8nz keeps one exception, bootstrap, "as small as it can be". N1, renting the machine, is the
provider's and stays in it. Whether N2 does too, and how small it can be, is the question; adding a
node is the one act of the platform's every node goes through, nine so far.

The ways in sight, and what each costs:

- **cloud-init user data, one per family**, pasted into the provider's form when the machine is
  rented: packages, Docker, Tailscale with a one-use auth key, `/data` formatted and mounted, then
  the node task as now. N2 shrinks to the paste. Against it: the house and a provider without
  cloud-init still go by hand; an auth key must be minted per node, by hand or by the platform
  holding Tailscale's API.
- **A prepared image per family and architecture**, booted rather than configured. Against it: a
  provider's support for custom images varies, and an image ages between builds.
- **The node task does more, the first run included**: installs what is missing and formats a disk
  it is told is `/data`. Against it: still a task over SSH from the laptop, RFC bz8nz's "Task".
- **A workflow from the console**, RFC dk6wz's "bringing it up", run once the machine answers on the
  tailnet. It can restart dockerd itself after `daemon.json`, live-restore making that safe, and set
  the tailnet name through Tailscale's API. Against it: it needs the tailnet first, so the login is
  bootstrap whatever is chosen, and a Tailscale token held by the platform.
- **N7 and N19 apart from N2**: keeper, which already restarts host, could restart dockerd when
  `daemon.json` changes; the tailnet name could be checked by host and set through Tailscale's API.

What would decide between them: whether every provider used and the house can take user data;
whether the platform should hold a Tailscale API key at all, minting auth keys and setting names,
or the tailnet stays wholly the author's; and whether restarting dockerd stays a person's act or,
under live-restore, becomes one a controller may take.
