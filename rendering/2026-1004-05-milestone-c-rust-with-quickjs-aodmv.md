---
name: 'Milestone C: Rust with QuickJS'
title: 'A server written in Rust serves the same artifacts, CTR only, and runs what a derivation computes in QuickJS'
status: accepted
supersedes: []
blocked-by: [adio2, c3j48]
commits: []
references: []
---

## Question

What does a backend that is not Node serve, and with what engine?

## Decision

**A server written in Rust serves the same artifacts, and runs what a derivation computes in
QuickJS.** seam's `spec/build.md` has why the artifacts do not change with the backend.

**There is no Rust backend without an engine.** QuickJS is in every Rust server, enabled by
default, and the two shapes Rust takes are this one and D's, with Node beside it. Nothing is built
for a Rust server that serves by walking the IR with no JavaScript, and nothing is decided for its
sake.

Accepted when:

1. **The same artifacts answer the same request with the same bytes** as the Node backend does.
   Node is held to Kit by A, so Rust is held to Kit through it.
2. **Rust starts no Node.** QuickJS is the only JavaScript engine in the process, and the check
   reads the process, not the configuration.
3. **A route Rust cannot serve is named at the build.** A derivation that reads what QuickJS has
   no host for -- `process`, seam's `spec/derivation.md` -- makes its route one only a
   JavaScript backend serves, said when the route compiles and never as a failure per request.

**Decided when C starts, not before: what runs the load stage.** Kit's `load`, `+server.js` and
remote functions are JavaScript run by Node, and on a backend that is neither TypeScript nor Node
something else has to answer for them. That is a question about the framework layer on another
backend, and C is where it is asked.
