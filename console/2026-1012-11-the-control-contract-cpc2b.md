---
name: The Control Contract
title: "The platform's control API answers every kind of resource in one shape the backend describes, and the console draws it"
status: accepted
supersedes: []
blocked-by: []
commits: []
references:
  - url: https://kubernetes.io/docs/concepts/overview/working-with-objects/
    took: 'an object as its spec, what is wanted, beside its status, what is, the two written by different parties'
---

## Question

The console is to operate every plane -- RFC dk6wz -- while its look is built apart from what it
operates. The author asked that the backend define what the frontend shows rather than the other
way round, so the console can be drawn once and follow whatever the platform comes to hold.

## Decision

- **Everything the platform operates is a resource of a kind** -- `node`, `app`, `binding`,
  `database`, and later `account` -- answered inside the `response` envelope like every other
  route, its `data` holding `kind`, `name`, `revision`, `spec`, `status` and `drift`.
- **`spec` is what is wanted and only a person writes it; `status` is what is and only a
  controller writes it.** Writing `status`, or a field the kind does not let be edited, is refused
  `forbidden_field`.
- **`drift` is computed by the platform and drawn by the console**, never computed there. It can
  be read off `spec` and `status`, and is kept beside them on purpose: whether a difference is a
  fault needs what a controller knows -- an app placed on a node of the wrong architecture, a node
  retired and still heard -- and the console computing it would be that knowledge twice.
- **A write carries the `revision` it was read at**, and one made after another change is refused
  409 `invalid_revision`, so two writers never silently overwrite each other.
- **What can be wanted is `spec`; an action is only an event with no state of its own.** Deploying
  a release, rolling back, placing an app, retiring a node and moving the database's primary are
  each a change to `spec`; backing up now or restarting are actions.
- **A write and an action answer 202 with `{"task": "<id>"}`**, a task in the ledger whose events
  are its progress and whose record is its audit, so the console draws every operation with one
  component.
- **The backend describes itself**: a catalog of the kinds lists each one's fields with their types,
  which can be edited, their choices and limits; its actions with their parameters and whether each
  asks for a confirmation; and the columns a list shows first. The console draws from the catalog,
  so a field or an action added in the backend appears without a change to the console. How it looks
  is the console's alone.
- **The console and the command line are two faces of the one API**, decided with the author on
  2026-10-13: each reads the catalog and draws from it, a form in the console and a command, `mise
run platform <kind> list|get|create|patch`, at the command line, so anything one can manage the
  other can, and neither knows a kind by heart.
- **A field the catalog marks `secret` is written and never read back**: the console draws it as a
  password field and the command line asks for it without echoing it. Its value goes to the secret
  service, RFC 231yq, and the resource keeps only the name it is kept under.
- **A field the catalog marks `masked` is an ordinary value drawn hidden until asked for**, decided
  with the author on 2026-10-13, when an account's identities had been made `secret` and the author
  could no longer read their own tenancy or email back: the resource keeps it and answers it, the
  console draws it as dots with a way to show it, and the command line prints it hidden unless told
  to show it. What a reader of the registry may not see is `secret`; what only the screen should not
  show at a glance is `masked`. An account's identity values are `masked`.
- **The version is in the path, `/v1/<kind>`**, and none is inside `data`: an answer is an envelope,
  and only a record kept at rest carries a `version`. A list is `{"items": [...], "revision": n}`.
- **The identity that writes is taken from the request, never from its body.**
- **Four codes were added to `response`'s catalog for it**, in its families: `invalid_revision`,
  `no_such_kind`, `no_such_node` and `forbidden_field`.
