---
name: Desired State Signed
title: "What a node is told to run is signed by the core and cut to that node, before any node or person but the author's joins"
status: accepted
supersedes: []
blocked-by: [jln34]
commits: []
references: []
---

## Question

Once host runs what the registry wants -- RFC jln34 -- the wanted state is root on every machine:
whoever can make a host read a forged one runs anything there. Today the registry's reads are open
to the tailnet, which every container reaches, and its writes take one shared token.

## Decision

- **The wanted state a node is given is signed by the core**, and a node checks it against the key
  the node task leaves on its disk at setup. Whoever carries it -- the registry, a neighbor's relay,
  the node's own cache -- can withhold it and cannot forge it, so trust no longer rides on the path.
- **A node is given only its own**: what it runs and who its peers are, read with its own identity,
  never the whole registry.
- **The wanted state is written by few principals and never by a node**: the console as the person
  Access names, and the operator's task. A node reports what is, through the relay, a path apart.
- **No node and no person but the author's is let in before this lands**: the platform is closed
  while it learns to change itself, since the quickest feedback is from its own author.
