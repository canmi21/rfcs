---
name: 'Stateless Steps, Stateful Ledger'
title: 'Whether a piece of work that takes many steps runs as stateless steps, its state held in the ledger alone'
status: accepted
supersedes: []
blocked-by: [cpc2b]
commits: []
references:
  - url: https://developers.cloudflare.com/workflows/
    took: "a run of steps, each step's result kept so a failed run resumes after the last one done, and a wait that holds no compute"
  - url: https://developers.cloudflare.com/durable-objects/api/alarms/
    took: 'one stateful coordinator per key, holding a timer, so polling is a wake-up and not a process kept alive'
  - url: https://cert-manager.io/docs/concepts/acme-orders-challenges/
    took: 'a certificate as a chain of resources -- certificate, order, challenge -- each with its own controller, none keeping state of its own'
  - url: https://docs.temporal.io/workflows
    took: 'state as the history of a run, and activities that must be idempotent because they can run again'
---

## Question

Much of what the platform does takes many steps and waits between them: adding a node, moving the
database's primary, and later issuing a certificate. Each is done today either by hand or by a
long-running process that keeps its place in memory, and a process that dies mid-way loses it. The
author asked on 2026-10-12 how to run such work the way Cloudflare's Workers run it: the code that
does a step keeps nothing and is gone once the step ends, and one stateful place decides what runs
next, holds the waits and keeps what came of it.

ACME is the example. An account is registered as the store says it should be. An order is made.
Each authorization is proved by a challenge, `dns-01` through a DNS provider's API or `http-01` on
a port. Then the order is polled until it is valid, finalized, and the certificate is fetched and
kept, in Postgres or in object storage. Whatever proved the challenge or polled the order does not
need to exist between those moments.

**What the model is made of:**

- **A step** is code given an input and an idempotency key, which returns an output and touches
  nothing but what it declares -- a database, a bucket, a DNS API, a secret. It keeps no state, so
  running it twice gives the same result, and that makes a retry safe.
- **A coordinator** is the only place with state. It knows what has been done, what runs next and
  when, and it decides that.
- **A wait is a timer the coordinator holds**, never a step kept alive. Polling an order means
  waking at a time, running one short step, and setting the next time.
- **A step is taken under a lease that expires.** A step whose runner dies is given to another runner
  once its lease ends, which is safe only because a step can run twice.
- **The unit deployed for a step is a function**: code plus a declaration of its bindings, its
  triggers and its time limit, run per call and destroyed after. Today's `service.toml` declares a
  service that stays running; a function would be another kind of declaration.

## Decision

Decided with the author on 2026-10-13.

- **Resources and steps each keep one role.** A resource says what should be true, such as a
  certificate valid for at least another thirty days. Its controller notices drift and opens a task,
  RFC cpc2b's `202 {"task"}`, and the task runs as a sequence of stateless steps. What is wanted is
  drawn before anything runs, from the resource; how it is being reached is drawn from the task.
- **A task's steps are code, replayed, in Cloudflare Workflows' API**: a `WorkflowEntrypoint` whose
  `run(event, step)` calls `step.do`, `step.sleep`, `step.sleepUntil` and `step.waitForEvent`, with
  `step.do`'s retries and timeout as Workflows declares them. A workflow written for the edge runs on
  a node unchanged, as a Worker does -- RFC l4s6a. A run is resumed by running its code again from
  the top, a step already done answering what it returned then, so code outside a step must decide
  the same way every time. The console draws a task's steps as they are recorded.
- **A workflow and its steps run in workerd's isolates**, RFC l4s6a, each step within a call's
  limits. Work longer than one call is more steps.
- **The ledger is the coordinator**: it keeps each task, its events, each step's result, the
  `run_at` a wait wakes at, and the lease a step is taken under, in the platform's Postgres.
- **How a lease is taken is declared by the workflow, by what losing or repeating its work costs**,
  as RFC bhybp classes every write:
  - **`quorum`, the default.** Leases and step results are kept on the primary, confirmed by two of
    the three cores, each lease carrying a fencing number. The scheduler on the primary's node takes
    each lease there and hands the step to a node, which sends back its result; the next step is
    handed out only once that result is confirmed. A node lost mid-step loses nothing: its lease
    runs out and another node takes the step. The platform's own work -- a change to `spec`, a
    membership, a certificate, a secret -- is always `quorum`.
  - **`local`, for work whose loss or repetition costs nothing.** The node a run began on schedules
    it: its leases and results are kept in that node's own store and sent on to the primary behind,
    idempotent on their key, with no round trip before a step. A node lost holds its runs until it
    returns, and a run lost with its disk is lost; a step may run twice.
- **Two kinds of lease, both supported**, decided with the author on 2026-10-13, as Temporal splits
  a workflow task from an activity task:
  - **A turn**, the default: a node is leased a run and replays it from the top, each `step.do`
    answered from the ledger when it is done and run when it is not, until a sleep, a wait, a retry
    or the end. Only replaying code knows the next step's name, so this is what keeps Workflows'
    code unchanged.
  - **A step**: a `step.do` that declares where it runs, `{ on: { role: "storage" } }` and the like,
    ends the turn on a node that does not match, and the step alone is leased to one that does,
    which runs its callback and records its result; the next turn replays on. Steps awaited together
    are leased apart, so they run on several nodes at once. Workflows on the edge ignores `on`, so
    the code still runs there.
    The first slice builds the turn; the tables carry a step's own holder and fence from the start,
    so the step lease adds no migration.
- **A lease is handed out by push and found by poll**: the scheduler pushes a granted lease to the
  node at once, and every node also polls every 15 to 30 seconds, so a push lost, or a node behind
  NAT, still finds its work.
- **The scheduler runs in the ledger on every core and acts only where Patroni answers `/primary`**,
  so it follows the database's primary with no election of its own; the ledger is placed on all
  three cores. A grant is one atomic statement that raises the run's fencing number, and a write
  carrying an older one is refused `409 stale_lease`. Leases run 30 seconds on the primary's clock,
  renewed every 10 and committed locally, since a lost renewal only shortens one; grants, results and
  outcomes are confirmed by two of three. A node lost mid-step is not counted against the step's
  retries, and a step started 20 times fails. New routes are `/v1`. The registry's own tasks stay
  one-event tasks until a controller opens a run.
- **A step's result is at most 1 MiB, kept in the ledger**, Workflows' own limit. A larger one goes
  to a bucket, RFC vh1bl, and the ledger keeps its content address. Until buckets are built, a step
  returning more than 1 MiB fails.
- **The second slice, the executor, decided with the author on 2026-10-14**:
  - **It runs on Node now and moves to workerd later**, split where RFC l4s6a will cut: a runner,
    `libs/replay`, that replays one turn with web APIs alone -- `fetch`, timers, `AbortSignal`, no
    `node:` module -- and a resident driver, `apps/compute/executor`, that polls, takes pushes,
    renews every 10 seconds and keeps to its capacity, each turn in a `worker_threads` Worker with a
    128 MB heap standing in for an isolate. A workflow imports `cloudflare:workers` and
    `cloudflare:workflows`, mapped to the platform's own modules under Node, and reaches services
    only through `this.env`. Once workerd runs functions the runner moves and the driver stays.
  - **A step's callback has at most 5 minutes**, l4s6a's limit, already under Node, so code written
    now runs in workerd unchanged; Workflows' default `timeout` reads as 5 minutes, and longer work
    starts asynchronously and is waited for.
  - **The node-placed step lease is in this slice**: `step.do(name, {on: {node}}, ...)` is leased to
    an executor on that node alone, as the step lease above, placed by node first and by role later.
  - **The first workflow is `mirror`**: after the database's base backup, each `store` mirrors it
    and the run ends when every one has. The store's 12:00 schedule stays as the fallback for two
    weeks of runs that end `done`, and is then removed.
  - **A push is a doorbell**: the executor polls for the turn it rings for, and the ledger pushes
    only to the executor on its own node, the rest finding theirs within the 15 to 30 second poll.
  - **An executor names the workflows it can run in each poll**, and a run is granted only to one
    that names its workflow, so a rollout never hands a new workflow to an old executor.
  - **Workflow code lives in the executor**, `workflows/<service>/`, until l4s6a's deployments carry a
    function's code apart; a run replayed by newer code that disagrees fails `replay_diverged` and is
    opened again by hand, each lease recording the executor's build.
  - **The executor reaches a node's socket services under the `scheduler` role**, as `cron` does,
    until a second holder needs less.
