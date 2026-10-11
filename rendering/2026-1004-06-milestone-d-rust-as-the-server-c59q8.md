---
name: 'Milestone D: Rust as the Server'
title: 'Rust is the server, and runs SSR in a Node process it starts for the parts of a route the artifact names SSR'
status: accepted
supersedes: []
blocked-by: [aodmv]
commits: []
references: []
---

## Question

What runs a route with an SSR part when Rust is the server?

## Decision

**Rust is the server, the meta-framework's server replaced, and the client is Kit's as it is.** SSR
runs in a Node process the Rust server starts and talks to, for the parts of a route the artifact
names SSR -- declared or degraded -- and nothing else.

Accepted when:

1. **The same application with the same declarations answers B's bytes.**
2. **Rust starts Node only where a route has an SSR part**, declared or degraded. An application
   none of whose routes has one is C, and C's second check holds of it.
3. **The client is Kit's**: the same client build A ships, unchanged by the server behind it.
