---
name: 'Apps Grouped by What They Do'
title: "The platform's apps are grouped by what they do, its libraries named for the role they serve, and the moves land in the one rebuild RFC zvrk7 already takes"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

RFC bz8nz took up the platform's own layout beside its running itself: the author asked whether the
platform's repository should be reorganized now that its scope had grown, its libraries grouped as
its apps are, and every name held to what it does. The apps sat in seven groups. `data` held nine,
mixing the platform's own records -- the registry, the secret service, the ledger -- with the
stores beneath them. `system` held the deployer beside the node's own agents, and the executor sat
in `compute` though RFC zlifc puts it in control. A survey on 2026-10-14 read how every tool finds an
app and measured with CI's own `deployable` what each move would rebuild.

It found that no deployed thing carries the group: every workflow, the image and Worker tasks, the
change graph and the registry import find an app by its leaf directory, while the leaf is carried
by its artifacts and their keys, its container, network and data directory, the role grants, Caddy,
the scopes, the registry and its database. So a regroup is a path change, and renaming a leaf is a
migration. The engine has no rename awareness, so a moved crate rebuilds its image, which is honest
since rustc embeds source paths in panic locations, and a moved pnpm package changes the lockfile,
which repackages every Worker. The whole set rebuilds about what RFC zvrk7's accepted full rebuild
already does. Two cross-repository readers break on a move: infra's `machine_setup.py` reads the
apt and apk units by path, and web's console reads the registry's catalog by path. Nothing holds an
app's directory to its `service.toml` `name`, though the artifact takes the one and the container
the other. And neither the Python nor the TypeScript engine rebuilds a Node image when a library it
imports changes.

## Decision

Decided on 2026-10-14, the author having left the layout to the lead.

- **Apps are grouped by what they do, never by layer.** The layer stays in each app's
  `service.toml`, where RFC zlifc reads it; infra's six apps are all ground, so grouping by layer
  would say nothing there. The platform's groups:
  - `control`: registry, ledger, secrets, deployer, executor -- the platform's own records and acts.
  - `data`: database, quorum, primary, postgres, objects, store -- what keeps the bytes.
  - `node`: relay, cron, apt, apk -- what every node is granted by default, the node's own agents.
  - `edge` (gateway, quota, router, hook), `delivery` (aka, cdn), `compute` (geo, shot), `observe`
    (probe, telemetry) and `model` (gemini, grok) keep their members.
  - `system` goes; no Worker moves.
- **Libraries are named for the role they serve and stay flat** until the workspace's grouping
  threshold is crossed; then `client/` holds what a program above the platform links and
  `service/` what only the platform's services share. `packages` becomes `steward`, the node's
  package agents' runtime; `replay` becomes `workflow`, the runner a workflow is written against.
  `probe` becomes `status`, the status database's schema, only with a published first version of
  the new npm name, which is the author's to release. `@monoflake/sdk` stays: its subpaths are an
  SDK web imports about 145 times, and only its URL root leaves; the Rust crate `monoflake` is
  removed with zvrk7's constants rather than renamed. `ledger` stays, the service's crate being
  `ledger-service` already.
- **infra's `libs/quota` becomes `disk-quota`**, since the platform's `quota` Worker is a rate limit.
  Caddy keeps its product name as the adapter for it, an exception to `spec/naming.md` recorded there.
  infra's libraries group into `node/` and `app/` once RFC 17b3o's four land.
- **No leaf is renamed now.** `store` to `backup` was the one worth it, and RFC vh1bl's buckets are
  likely to absorb it.
- **A preparation lands first and rebuilds nothing**: the two cross-repository readers find the apps
  they read without the group in the path, a gate refuses an app whose directory is not its
  `service.toml` `name`, and the engine rebuilds a Node image when a library it imports changes.
- **The moves and the library renames land in zvrk7's rebuild push**, so the platform is rebuilt
  once rather than twice; the manual apps among them -- registry, ledger, secrets -- may stay
  unrolled until their next release. Should that rebuild be far off, the Rust moves go alone (seven
  images, no Worker) and the Node moves and renames once after (three images, every Worker).
- **Whether cron belongs to the ground** -- it triggers the database's backup, which zlifc keeps in
  the ground, yet links the ledger's client and is not on zlifc's list -- is a question of its own,
  a draft.
