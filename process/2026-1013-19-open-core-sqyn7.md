---
name: 'Open Core'
title: 'Which parts of the platform are published as open source, so their CI runs free, and which stay private'
status: draft
supersedes: []
blocked-by: [q28g5]
commits: []
references: []
---

## Question

GitHub Actions is free in public repositories and metered in private ones: the free organization
includes 2,000 minutes and 0.5 GB a month. October's 6,654 minutes cost nothing only because
`monoflake/platform` and `monoflake/infra` were public for most of it; once private, the storage ran
out on 2026-10-13 (RFC q28g5 moved the images to a bucket) and the minutes will run out next. The
author asked on 2026-10-13 whether part of the platform could be published, as Tailscale publishes
its client and keeps its coordination server, so that what builds most runs free while the control
plane stays private. Decided to wait until the platform runs.

## Options

**The leaning, not decided:**

- **Published**: the node agent -- host, keeper, meter, `libs/deploy`, `libs/quota` -- the client
  libraries -- the ledger client, `libs/sdk` -- and the services that stand alone: shot, probe, geo,
  the apt and apk mirrors, cron. The heaviest builds are among them, shot's browser image and the
  agent's two architectures.
- **Private**: the control plane -- registry, ledger, secrets, deployer, gateway, scheduling -- the
  database and quorum tooling, and every inventory: `nodes.toml`, the firewall rules, the sops files,
  the tasks that act on nodes.
- **Each published part starts a new repository with a new history**: `infra`'s history holds the
  nodes' addresses, so it is never made public as it is.
- **A public repository's CI deploys only on a push to `main`, never runs on a self-hosted runner,
  and writes the bucket with a key limited to its own prefix**, so a fork's pull request reaches no
  secret and no machine, and a public workflow cannot replace a private image.
- **The license is open**: an open-source license, MIT, Apache-2.0 or BSD as Tailscale's client
  uses, or a source-available one, FSL, BSL or ELv2, that turns open after a delay and forbids a
  competing hosted service.
- **Published code is reviewed for what it assumes is unseen** -- tokens, the firewall's design, the
  node's authentication -- before it is.
