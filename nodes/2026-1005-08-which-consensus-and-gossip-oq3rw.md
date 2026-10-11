---
name: Which Consensus and Gossip
title: "Which libraries carry the relay's consensus and its gossip is open until they are measured"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The relay's core agreement is a consensus problem and its spreading an anti-entropy one, and neither
is to be written from scratch -- platform's `spec/architecture/relay.md`. What each candidate costs
on a node with a gigabyte of memory, and whether one library could do both, is undecided until they
are measured.

## Options

| For       | Candidate | What it is                                                              |
| --------- | --------- | ----------------------------------------------------------------------- |
| Consensus | openraft  | Raft in Rust                                                            |
| Gossip    | chitchat  | Quickwit's scuttlebutt gossip, which compares versions to find who lags |
| Gossip    | foca      | A SWIM implementation                                                   |

None has been tried here.
