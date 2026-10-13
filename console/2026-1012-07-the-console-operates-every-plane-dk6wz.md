---
name: The Console Operates Every Plane
title: 'Every act that changes a node, the platform or an app is done from the console, a task over the same API kept as the way in'
status: accepted
supersedes: []
blocked-by: [ja5hm, qcyb3]
commits: []
references: []
---

## Question

The console reads and does not write; what changes the system is a `mise run` task, run by the
author or by an agent on the author's machine. Ship cloud cannot manage itself, and the platform's
database, on the three cores since 2026-10-12, holds nothing the console could write to.

## Evidence

What bringing `sin` up and making it a core took on 2026-10-12, all of it by hand: the node task, the
tunnel task and the first host; every node's peers and hosts recreated; the deployer's grants edited
on `tyo` and the deployer redeployed; the database's environment written, a switchover, an etcd
member removed, one added and promoted, the database and quorum deployed on the new member, two
standbys and every node's database proxy redeployed; and a run dispatched by hand for one Worker.
Before it, the primary had been the emulated `buf` for a day and a half, and nothing said so.

## Decision

- **Every act that changes the system is done from the console**: on a node -- bringing it up, its
  tunnel, its first host; on the platform -- where an app runs, its labels, what a repository is
  granted, the database's switchover and members; on an app -- deploying, redeploying, rolling back,
  stopping and starting, its environment and secrets.
- **A write goes where RFC ja5hm puts it**: what the platform holds to its store, an act on a running
  app to that node's host.
- **A task stays the way in when the console cannot be reached**, calling the same API, never a second
  path that writes differently.
- **Only the author writes, behind Access, until accounts exist**, so this waits on no account system.
- **First what a new node took**, since that is the act done most by hand: the registry, placement,
  grants. Then the database's acts, then environment and secrets.
- **What the console shows says what is wrong without being asked** -- a primary on a node that
  should not lead first.
