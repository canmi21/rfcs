---
name: 'The Platform Runs Itself First'
title: 'The platform is self-hosted when every act on its own services and nodes goes through the registry and its controllers, and no app is brought onto it before then'
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

By 2026-10-14 the platform placed its services through the registry (RFC ja5hm), deployed them
from CI artifacts by host and the deployer (RFC jln34), and recorded each release; and it was
running the first steps of its scheduler (RFC z9vqb). Much else was still done by hand: a service's
configuration edited over SSH in its `config.env`, a secret moved with sops and a pipe, a database
and a bucket made by hand, a Worker's token set from a laptop, a node brought up by a task run
locally. Each piece of the platform had been taken partway, and none of the partial steps said how
far the whole had come. The author asked how to reach the platform they intend, observing that the
platform did not yet qualify to run software above it. Bringing an app onto it to find the gaps
was weighed and refused: the platform's own design had not settled, so an app would meet the
platform's unfinished parts rather than the edge cases it exists to find.

## Decision

Decided with the author on 2026-10-14.

- **Self-hosted has one test.** Every act on the platform's own services and nodes -- deploying,
  rolling back, configuring, giving and rotating a secret, making a database or a bucket, placing,
  adding a node, deploying a Worker, routing a name -- is done through the registry and the
  controller that brings what runs toward it, started from the console or from a command on the one
  control contract (RFC cpc2b and dk6wz). None needs SSH, a file edited on a node, sops by hand,
  wrangler or gh from a laptop, a GitHub secret set by hand, or a provider's dashboard. Bootstrap is
  the one exception, named and kept as small as it can be: what must exist before the registry does.
- **Progress is counted, not described.** The inventory of every act done by hand is kept, each with
  how it is done today, how often, and the capability and RFC that replace it. How self-hosted the
  platform is, is how many rows are left. An act with no RFC gets one before it is worked on.
- **No app comes onto the platform until the count reaches bootstrap alone.** The site's move (the
  roadmap's first app) and any new app wait. The platform's own services are its first users, and
  the gaps are found on them.
- **The gaps close in the order that removes the most hand acts first**, each capability built as the
  ones already done were built: a field in the registry, and a controller converging on it.
- **The platform's layout and names are reviewed against this scope**: its apps grouped by plane after
  RFC zlifc's layers, its libraries grouped by who imports them, and each name held to what it does
  -- a decision of its own, taken from the survey of what a move costs.
