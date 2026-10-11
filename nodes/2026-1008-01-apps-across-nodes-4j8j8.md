---
name: Apps Across Nodes
title: 'An app reaches an app on another node only by a tailnet address written by hand, and what names it is open'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Found on 2026-10-08, when cue's `qq` on `sha` had to reach its `hub` on `tyo` over a long-lived
WebSocket: nothing names an app across nodes. The only way that works is the `peer` role -- the
hub's port published on `tyo`'s tailnet address, admitted from the tailnet alone -- with that
address written into `qq`'s `config.env`, so the hub moving to another node is an edit by hand.
`peer` was meant for an app talking to itself on other nodes, and is borrowed here. The private
suffix does not help: the house's resolver answers it with the house's node, for the LAN.

## Options

| A name each app is reached by from any node                          | Against it                       |
| -------------------------------------------------------------------- | -------------------------------- |
| A proxy on every node by app name, as `primary` is for the database  | A hop on every cross-node call   |
| The private suffix resolved on every node to the node an app runs on | A resolver kept current per node |

Either is fed by the same knowledge of which node runs what that the public router needs.
