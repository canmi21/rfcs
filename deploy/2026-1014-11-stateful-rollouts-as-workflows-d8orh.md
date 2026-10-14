---
name: 'Stateful Rollouts as Workflows'
title: 'A release of the database, the quorum, the registry, the secret service or the ledger is rolled a member at a time by hand, and whether a ledger workflow rolls it, a Postgres major with it, is open'
status: draft
supersedes: []
blocked-by: [23wgz, jln34]
commits: []
references: []
---

## Question

Five apps are `rollout = "manual"`: the database, the quorum, the registry, the secret service and
the ledger. A run that builds one deploys nothing of it; the operator runs `mise run node deploy
<node> --run R --repository X --app A` once per member over SSH, standbys first, a switchover
before the leader so its restart fails nothing over, each answering healthy and caught up before
the next -- infra's `spec/architecture/host.md`, "An app chooses how it is rolled out", and
platform's `spec/architecture/databases.md`, "Upgrades are pinned, reported, and rolled by hand".
The task, `infra/.mise/tasks/by_hand.py`, then writes the release to the registry, forward only,
since a manual app's release is the operator's (RFC jln34, since 2026-10-13).

It is row D3 of platform's `spec/architecture/operations.md` and the most frequent hand act of
2026-10-13 and 14: the ledger was rolled through the three cores twice on 2026-10-14, and bringing
`sin` up on 2026-10-12 deployed the database and the quorum on a new member by hand -- RFC dk6wz's
evidence. No RFC covers it.

Row D12 is the rarer half: a Postgres major, at most once a year, rolled by logical replication in
four steps -- a standby emptied onto the new major and subscribed, writes stopped while sequences
are carried and it becomes primary, then the other standby and the old primary copied from it -- a
new backup chain under a prefix of its own. The proxy on every node means the switch rewrites no
app's URL since 2026-10-08.

The parts exist. RFC 23wgz's ledger keeps a run's turns, leased and fenced on the primary, and its
executor replays them on four nodes since 2026-10-14, with a step that can be placed on a named
node; host deploys a run on notice; Patroni switches over on request. What is new is the circle:
the ledger is one of the five, and it keeps its state in the database, another of them, so a
workflow rolling either restarts what it runs on.

The ways in sight, and what each costs:

- **One workflow, `rollout`, for every manual app**: a step placed on each member asks its host to
  deploy the run and waits for it healthy and caught up; a switchover step stands before the
  leader's; the order is declared in the app's `service.toml`. It is started by the registry's
  wanted release changing, RFC jln34, or an action from the console, RFC cpc2b. Against it: the
  ledger must carry a run through restarts of its own members and through the primary moving under
  it. The parts say it should -- three ledgers, a scheduler following Patroni's primary, a lost
  lease taken by another node -- and nothing has proved it.
- **host rolls them itself**, a fourth `rollout` beside `replace`, `beside` and `manual`, each node
  waiting its turn through the relay, needing no ledger and so no circle. Against it: host knows one
  node, and which member leads is Patroni's; the order and the switchover would be taught to host
  for five apps.
- **The task kept, run from a core rather than the laptop.** Against it: it is still SSH, which RFC
  bz8nz forbids whoever runs it.
- **A major as a workflow of its own**, or kept a written procedure through its first run and encoded
  after, since it comes once a year and has not been done.

What would decide between them: whether a ledger workflow can roll the ledger and move the
database's primary under itself, tried first on the quorum, which the ledger does not stand on;
whether the member order and the switchover belong in the declaration; and whether a yearly major
is worth encoding before it has been run once by hand.
