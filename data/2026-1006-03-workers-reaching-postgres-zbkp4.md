---
name: Workers Reaching Postgres
title: 'How a Worker at the edge reaches Postgres on the nodes, direct or through Hyperdrive, is open until measured'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

A stateless Worker in any of Cloudflare's locations asking one Postgres on a node pays the distance
on every query. Which way it connects, and what is cached at the edge, is undecided until a query is
measured.

## Options

| How a Worker reaches Postgres | What it is                                                          |
| ----------------------------- | ------------------------------------------------------------------- |
| Directly, over Workers VPC    | TCP to the node, every query paying the distance                    |
| Through Hyperdrive            | Pooled and cached connections in front of the same private database |
