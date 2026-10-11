---
name: A Node Offline Misses Runs
title: 'A node that was offline misses the runs that ended meanwhile, and whether host asks GitHub on start is open'
status: landed
supersedes: []
blocked-by: []
commits: [infra@8d8c047e, infra@d7653540]
references: []
---

## Question

A finished run reaches a node only as a notice, which the platform's `hook` sends once, when GitHub
calls it -- infra's `spec/architecture/host.md`, "The machine pulls; nothing pushes into it". GitHub
does not deliver a webhook again on its own, so a node that was down then never learns of the run,
and keeps the version before it until the next push to the same app. With a node that is allowed to
go offline -- infra's `spec/architecture/nodes.md`, `home` -- that is no longer rare.

The direction is the relay's ordered log, which a node that was away catches up on by comparing its
place with any neighbor's -- platform's `spec/architecture/relay.md`. Whether host also asks GitHub
on start, until the relay exists, is undecided.

## Decision

**host asks GitHub, at start and every fifteen minutes, until the relay carries the log.** It
lists each source's runs to deploy from the last seven days and takes the ones it has no record of,
newer than the oldest it holds, each deploying only the apps no newer run built; a missed run that
built host goes to keeper -- infra's `spec/architecture/host.md`, "The machine pulls; nothing pushes
into it".
