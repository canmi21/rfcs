---
name: 'Lower Layers Never Call Up'
title: 'Every app names its layer, a lower one never calls a higher one at run time, and the ground is kept to what repairing the platform needs'
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The platform is built so the author maintains less: overlay databases and buckets that back
themselves up, a ledger that runs workflows, a registry that says what runs where. Yet the ground
under it -- host, keeper, the relay, the meter, the database cluster -- cannot lean on any of that,
since a ground that needs the platform to run cannot repair it when it is down. The author asked how
the ground stays below everything and still gets the most from what is built above, since today
the ground is the larger burden.

The day it was asked, 2026-10-14, the burden came from glue rather than from layering. Moving the
artifact bucket from B2 to R2 touched six places that each wrote B2's endpoint; `artifact.ts` is
kept identical in platform and infra by hand; the address map needed two sources, since the
published `@monoflake/urls` lagged; the registry task found `DEPLOY_SOURCES` by parsing infra's
node task as Python, and broke when that task was split; and a B2 quirk, versions kept under a
delete, had to be patched in our code.

## Decision

Decided with the author on 2026-10-14.

- **Every app names its layer in its `service.toml`**, one of five, each calling only those below
  it at run time:
  - `ground`: what runs a node and shows it -- host, keeper, Caddy, the tunnel, the resolver, the
    relay, the meter.
  - `store`: the bytes themselves -- the database cluster and the buckets the platform's data is
    kept in, reached as a provider gives them.
  - `control`: the platform's own services -- registry, ledger, secrets, gateway, deployer, the
    executor -- on the store directly.
  - `overlay`: what an app is given -- the databases and buckets of RFCs 6jw04 and vh1bl -- whose
    own records are kept in `control`, never in themselves.
  - `app`: everything else, the console among them.
- **The ground is what repairing the platform needs, and nothing more.** The test for an app is
  one question: with every service above the store down, is it needed to deploy, reach, see or
  restore the platform? Only then is it `ground`. What sounds like infrastructure and fails the
  test is built above, where it can use all of it: the meter's history past its days on the node,
  usage and quotas, sending an alarm the node judged, archiving logs, backing up anything but the
  database. The list of `ground` apps is written in infra's spec, and adding one is an RFC.
- **A lower layer uses a higher one in four ways, none of them a call it waits on:**
  - **Code, not a service.** What an upper layer is good at is written as a library first, and
    the ground links it -- `libs/deploy`, the address packages.
  - **Wanted state from above, kept below.** An upper layer decides and writes what should be; the
    ground reads it, keeps the last it read, and acts on that while the layer above is down, as
    host acts on the registry's wants.
  - **Data up, never a dependency up.** The ground sends what it holds to a higher layer when it
    can -- the meter's minutes into an overlay database, once there is one -- and loses nothing it
    needs when it cannot.
  - **The upper path first, a plain one beside it.** Where the ground uses a higher layer for
    convenience, a plain path that needs only the ground and the store stays, used when the other
    fails, and is tried in a drill; `mise run node` deploying without the platform is the first.
- **The check enforces it, built on what already holds every address.** The address map already
  names every URL once, and `mise run refs` refuses one written anywhere else. Each entry in it,
  and each binding a service declares, names the app it reaches; the check reads every app's
  layer and refuses a run-time reference from a lower layer to a higher one. A reference only a
  build or a task follows -- CI, `mise run` -- is not a run-time one.
- **One fact, one source; across repositories, an interface, never an implementation.** A value
  written twice is a value that drifts; a repository reading another's source breaks when that
  source is reshaped. The first to fix, in this order:
  1. The address map's two sources: `@monoflake/urls` is published with what infra adds, and
     platform's map takes it from there.
  2. `artifact.ts`, kept identical by hand: one is generated from the other, or both from a
     package, and the check refuses a drift.
  3. The registry reading `DEPLOY_SOURCES` from infra's Python: infra writes it where it writes
     its other data, and the registry reads that.
- **A provider is chosen with the code it makes us write.** A quirk that has to be patched around,
  as B2's versions were, counts against it beside its price.
