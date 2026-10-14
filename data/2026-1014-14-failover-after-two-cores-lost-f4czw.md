---
name: 'Failover After Two Cores Lost'
title: "With two cores lost the database waits for a person's forced failover, which has no path but a shell in its container, and what guarded act replaces the shell is open"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Only a core holding every confirmed commit is promoted: one core lost fails over by itself, but two
lost at once -- Asia whole, `tyo` and `sin` -- leave `rdu` waiting for the operator's `patronictl
failover --candidate <node> --force`, which accepts losing what only the lost cores held --
platform's `spec/architecture/databases.md`, "Where it runs, and which one writes", since RFC
bhybp. That a person decides is by design. That the only way to decide is a shell inside the
database's container on a surviving node is not; it is row DA3 of platform's
`spec/architecture/operations.md`, with no RFC.

The same loss asks for a second person's act: the secret service cannot unseal on one core, and the
author opens it with the offline recovery key, `mise run seal unseal <node> <identity>` (row S9,
RFC 231yq). It is a task over SSH, kept the person's on purpose.

What makes the case unusual is what is down when it comes. With no primary the registry and the
ledger cannot write, so RFC cpc2b's write -- a change to `spec` answered by a task in the ledger --
cannot be recorded; and with the secret service sealed, nothing reads a secret. What still answers
is host on every surviving node, the relay, and the quorum, which keeps three of five voters when
Asia is gone.

The ways in sight, and what each costs:

- **A host action on the database's app**, `failover` with its candidate and a confirmation typed
  back, sent to the surviving core's host by `mise run node` today and by the console's path to host
  once RFC 2p4a1's is built, recorded in host's own history rather than the ledger. Against it: host
  learns one app's verb, and an action that needs no database must be reachable without the
  registry, which the console reads.
- **A cpc2b action recorded once the database is back**: taken while it is down by whatever answers,
  written to the ledger after. Against it: two paths for one act, which RFC dk6wz refuses.
- **Kept a shell, named as break-glass** beside D10's archive deploy, with a written procedure in
  `databases.md`. Against it: RFC bz8nz counts it as hand for good, and the act it guards is the one
  most likely to be done in haste.
- **The unseal with it**: S9 and DA3 are one incident, so one guarded path for both -- the failover
  first, then the unseal, the console saying which is next -- or each left on its own.

What would decide between them: whether an act taken when the platform's state store is down may be
recorded anywhere but the ledger; whether host should know any app's own verbs; and whether a person
should be shown, before confirming, what the forced failover loses -- the newest standby's lag
against the lost primary's last position -- which only the surviving members can say.
