---
name: 'Worker Secrets and Rollback'
title: "A Worker's secrets are put, and a Worker rolled back, by wrangler from the laptop, and whether the deployer does both as host does them for an app is open"
status: draft
supersedes: []
blocked-by: [231yq, 4s460]
commits: []
references: []
---

## Question

The deployer deploys a Worker from its run's artifact as host deploys an app, platform's
`spec/architecture/deployer.md`; the console is on it since 2026-10-07, and RFC 4s460 moves the rest.
Two of what host does for an app it does not do for a Worker, rows of platform's
`spec/architecture/operations.md` with no RFC:

- **W4, a Worker's secrets.** "Secrets survive deploys, so a Worker's secrets are set once by hand
  until they come from sops" -- deployer.md. Today that is `wrangler secret put` from the laptop:
  the hook's webhook secret, the gateway's tokens, the console's `REGISTRY_TOKEN`, the last wrapped
  by `mise run registry token`. A shared node secret rotated, row S7, is a `wrangler` call per Worker
  that holds it. RFC 231yq holds the value; who hands it to a Worker is not written.
- **W5, a Worker rolled back.** `wrangler rollback`, any of Cloudflare's last hundred versions,
  "offered as host offers it" -- deployer.md, "Rolling back". host rolls an app back from the
  version before, and RFC jln34 makes a rollback the registry's wanted release set back, which the
  deployer, reading the registry, would follow for a Worker as host does for an app.

The deployer already holds the account's Workers token and records each deploy's version. What it
lacks is a source for secrets, and a rule for where a rollback goes: the artifact bucket keeps two
runs, RFC q28g5, while Cloudflare keeps a hundred versions.

The ways in sight, and what each costs:

- **Secrets: the deployer reads a Worker's from the secret service and puts each changed one** on
  every deploy and when one changes, as host writes an app's `secret.env`; a Worker's declaration
  names the secrets it reads, as an app names its addresses, RFC zvrk7. Against it: the deployer
  reads every Worker's secrets, so its token reaches every scope that has one.
- **Secrets as bindings of a Secrets Store**, Cloudflare's, written by the platform and bound by
  name, so a rotation is one write and no redeploy. Against it: a second place a secret lives, and
  one more Cloudflare product under the platform.
- **Rollback as the wanted release set back**: the console sets it, the deployer redeploys that run's
  artifact. Two runs back at most, unless the bucket keeps more for Workers, and the secrets and
  bindings of today, not of then.
- **Rollback as Cloudflare's version**: the deployer calls Cloudflare's rollback to a version it
  recorded deploying, a hundred deep and immediate. Against it: the registry's wanted release and
  what runs disagree until the next deploy, and the next round of a controller undoes it unless the
  wanted release moves with it.

What would decide between them: whether a Worker's secrets may be read by one service for every
Worker, or each scope's by a token of its own; whether a rollback deeper than two runs is wanted
for Workers; and whether a rollback must also put back the secrets and bindings the old version ran
with.
