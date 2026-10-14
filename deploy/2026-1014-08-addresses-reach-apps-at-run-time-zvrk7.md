---
name: 'Addresses Reach Apps at Run Time'
title: "An address a running app dials is given to it by host as an environment variable it declares, from infra's one map, so moving a host restarts apps rather than rebuilding them"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Every address is declared once, in the address map -- the workspace's `spec/addresses.md` -- and
read as a compiled constant: platform's `libs/sdk` turns the map into Rust `pub const`s and a
TypeScript module, and eleven Rust images, two Node services and six Workers are built against it.
On 2026-10-14 the artifact bucket moved from B2 to R2 (RFC 7cwmp); changing that one endpoint
rebuilt eleven images and six Workers, a gigabyte of artifacts, though nothing in them changed but
a string. The map also has two sources, infra's `libs/urls` published as `@monoflake/urls` and the
platform's copy, because the published package lagged. The author asked for the addresses to leave
the build and be given to the apps at run time, accepting one full rebuild to get there.

## Decision

Decided with the author on 2026-10-14.

- **Only what a running app dials, and might move, is given at run time**: our own hosts -- the
  ledger, the private and public API, shot, host, keeper, the panel, the console's -- the artifact
  bucket, and the public hosts the probe and aka reach; about fifteen entries. A third party's API,
  a link to documentation, an identity and a development port stay compiled: they change only with
  a rename, which is a change to code anyway.
- **An app declares the addresses it reads, as a `[urls]` table in its `service.toml`**: each an
  environment variable named for the map's key. host writes their values into the app's
  environment beside the `config.env` it writes already, as `LEDGER_URL` is given today. An image
  carries no default: a variable missing stops the app at start, rather than let it dial an old
  address. A file holding the whole map was weighed and not taken: no app needs the whole map, and
  none needs a change without a restart.
- **The declaration is also the layer check's input.** RFC zlifc refuses a run-time reference from
  a lower layer to a higher one; `[urls]` names, for each app, every address it reaches, so the
  check reads the declarations rather than the code.
- **One source, infra's `libs/urls`**, holding every run-time entry, the platform's hosts among
  them, since where a service runs is infra's to say. It stays published as `@monoflake/urls`; the
  hosts are public in DNS already. host compiles it in and is the one reader of the values on a
  node; the platform reads the package for its keys alone, so a lag in publishing no longer stands
  between a change and the nodes.
- **A Worker is given the same values by the deployer**, as `vars` at deploy, from what host gives
  the deployer. The deployer records the map each Worker was deployed with and deploys it again,
  from the artifact it holds, when the map changes.
- **A changed address costs a restart, not a build**: one commit to infra, host rebuilt and rolled
  out canary first, and each app whose given values changed rolled -- without a gap where it runs
  beside itself -- on the image it already has; then the Workers.
- **web is not moved now.** Its addresses are links and identities, none a host we would move;
  revisited when one is.
- **The order**: the map gains the run-time entries and is published, the platform's constants kept
  one release and marked to go; host learns `[urls]`, writing nothing until an app declares it; the
  one rebuild, each image reading its environment, rolled one at a time; the deployer and the
  Workers; the platform's constants removed and each repository's `refs` held to the new home; the
  layer check reading `[urls]`.
