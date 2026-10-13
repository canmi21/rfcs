---
name: 'Stateless Steps, Stateful Ledger'
title: 'Whether a piece of work that takes many steps runs as stateless steps, its state held in the ledger alone'
status: draft
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

## Options

There are two ways to keep the state, and they hold different things.

- **A run's history (Workflows, Temporal).** The state is the steps done so far and what each
  returned, and a run resumes after the last one. This suits a sequence with an order and with
  waits, such as an ACME order or adding a node.
- **A chain of resources, each with a controller (cert-manager).** The state is each resource's
  `spec` and `status`; a controller reads one, moves it a step and writes `status` back. Nothing is
  lost when a notice is lost. This is already the shape of RFC jln34's controllers and of RFC
  cpc2b's contract, and the console draws it as it is.

**The leaning is both, each in one role.** A resource says what should be true, such as a
certificate valid for at least another thirty days. Its controller notices drift and opens a task,
RFC cpc2b's `202 {"task"}`. The task runs as a sequence of stateless steps. **The ledger becomes the
coordinator**: it already keeps tasks and their ordered events for good. It would gain a step's
lease, taken with `FOR UPDATE SKIP LOCKED`; a `run_at` timer; and what each step returned, all in
the platform's Postgres on the cores. That is the platform's scheduler, which
`spec/architecture/scheduling.md` describes and nobody has built, and the execution layer RFC dk6wz
needs: what adding `sin` took by hand on 2026-10-12 is such a sequence written down.

**Still open:**

- Whether a task's steps are declared as data, so the console can draw a step before it runs, or
  written as code that is replayed.
- What runs a function: a container started per call on a node, a long-lived runner that pulls
  steps, or Workers at the edge for a step that needs no node.
- Whether the ledger's single writer, RFC bhybp, is fast enough for leases taken from every
  continent.
- How a step's output is bounded: a small result kept in the ledger, and anything larger in object
  storage, with only its reference kept in the ledger.
