---
name: 'Being Told Something Is Wrong'
title: 'Nothing tells the author when something is wrong -- an alarm, a failed deploy, a stalled backup, a sealed service -- and who judges, through what channel and how loudly is open'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The platform judges a great deal and says it to nobody. The relay judges each node's alarms from
its minutes -- processor, memory, storage, temperature -- and carries them in the node's snapshot,
platform's `spec/architecture/relay.md`, "A node's readings stay on the node"; host records a
failed deploy; the ledger records a failed run; the deployer records a failed Worker deploy where
the console does not read it, RFC yu4kw; a database that has stopped archiving says so on its
`/health`, which nothing outside the node asks, RFC np65b. Each is seen when the author looks. It is
row O3 of platform's `spec/architecture/operations.md`, with no RFC; infra's roadmap names
"Notifications", and infra's `spec/architecture/host.md` puts them "after the first version".

How long a fault waits for a look has been measured: the primary sat on the emulated `buf` for a
day and a half before 2026-10-12 and nothing said so -- RFC dk6wz's evidence; `gvx`'s host held a
dead socket for five hours on 2026-10-14 after the weekly upgrade restarted Docker. RFC bz8nz has the
platform run itself before any app comes onto it; this is what makes leaving it unwatched safe.

What deciding it involves, and the ways in sight:

- **Who judges.** Each source sends its own, from the relay, host, the ledger and the deployer; or
  every source's records feed one judge that decides what is worth saying, keeps the open ones, and
  says when one clears. One judge is one place to silence, group and repeat; it is also one more
  service that must be up to say that services are down.
- **Where it runs.** On the nodes, a relay's leader or the ledger's scheduler, which then cannot
  report its own node lost; or outside the platform, as the probe on `rdu` reports the platform from
  outside -- a Worker, or the status page's own place -- which sees a node gone but not inside it.
- **Through what.** Mail, through Cloudflare's sending; a push to the author's devices; a chat
  message through a bot, which then must be up; a webhook any of them takes. Each channel needs its
  own credential and its own failure path.
- **How loudly.** Severities, a quiet time, a repeat until acknowledged, and an acknowledgment
  written back -- the console showing what is open and who saw it -- or a plain message per event.
- **What is in from the start**: the relay's alarms, a failed deploy, a run failed, a backup that
  stopped, a sealed service, a node not heard; and the console's own list, which RFC dk6wz asks to
  say what is wrong without being asked.

What would decide between them: whether a notice must still arrive when the cores are gone, which
puts its judge outside them; which channel the author reads first at any hour; whether an alarm
needs acknowledging or only telling; and whether RFC np65b and yu4kw are folded into this or stay
the first two sources it serves.
