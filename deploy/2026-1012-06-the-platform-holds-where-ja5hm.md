---
name: The Platform Holds Where
title: "Git holds what an app needs, the platform's Postgres holds where it runs and with what, and the console writes it there"
status: accepted
supersedes: []
blocked-by: []
commits: []
references:
  - url: https://developers.cloudflare.com/workers/runtime-apis/bindings/
    took: 'a binding named in the code, and the resource it points at held by the platform and handed over at deploy'
---

## Question

Bringing `sin` up on 2026-10-12 took commits in three repositories and an edit by hand on `tyo`: every
everywhere-placed app's `placements`, four Workers' VPC service ids, the console's facts and places,
the deployer's `WORKER_RESOURCES`, and every node's peers, the database proxy's members and the
egress proxies, each read when a process starts, so every node restarted something. Git was the one
record of all of it, and a change made in the console was to become a pull request a bot opens.

The author asked for what Cloudflare does instead: the code names a binding, and the platform holds
what it points at and hands it over when it deploys.

## Options

- **Git stays the record, the console opens pull requests.** Every change is a commit and a run, a
  node is still added in every place that lists the nodes, and what a process reads at start still
  needs a restart.
- **The relay's log holds it**, signed by the core and copied to every node. It is not built; the
  database is.
- **The platform's Postgres holds it**, on the three cores and failing over by itself since
  2026-10-12, its stability judged settled by the author.

## Decision

- **Git holds what an app needs; the platform holds where it runs and with what.** In git: its image
  or Worker, ports, memory, health, routes, the bindings it asks for by name, and what it cannot run
  without -- an architecture, a role. In the platform: the nodes and their facts, where each app
  runs, its labels and domains, what each binding resolves to, each node's peers and the members it
  routes to, and what a repository is granted.
- **It is kept in the platform's Postgres**, the node registry included, in place of infra's state
  kept in git and the relay's log.
- **The database unreachable is a freeze, never an outage.** Every node keeps on its own disk what it
  last read and runs on it; nothing reads the store to keep running, only to change. A change waits
  until the store answers.
- **A change is a row appended, never one overwritten**: who, when, what was and what is. It is the
  history git gave, and any earlier state can be read back and put back.
- **A change reaches what reads it while it runs.** Peers, a proxy's members, the egress proxies and
  the console's nodes are read again when they change, and nothing restarts for them.
- **The console writes it directly**, with no pull request and no bot, in place of RFC sul8l, declined.
  Where an app runs is set there, within what its declaration requires, rather than declared in the
  repository as a group, in place of RFC 15m3x, declined.
- **What git holds of the second kind is imported once and then removed from git** -- `nodes.toml`,
  the `placements`, the VPC ids in each `wrangler.jsonc`, the console's list of nodes. A Worker's
  bindings are written into its config by the deployer when it deploys, from the store.
- **The database's own members stay named by hand** in its declaration, since the store is that
  database. **What a node needs to reach the store at all** -- the tailnet, its host, its tunnel's
  token -- is set up by the node task and kept on its disk, as it is now.
