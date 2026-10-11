---
name: Grok Waits for Sign-In
title: 'grok binds no port until it is signed in, so a first deploy reads as failed, and serving health at once is open'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

A signed-out grok2api waits for `grok2api login` and binds no port until it is signed in, which
suited a container somebody could sign in inside. Under host a deploy is judged by its health check,
so a first deploy of a signed-out app reads as failed, and the sign-in has to come first, by hand --
platform's `spec/architecture/grok/deployment.md`, "Under host the first sign-in comes before the
first deploy".

Serving `/health` at once, and answering the API with a 503 until the agent is ready, would remove
the step; it changes what the server promises a caller, which is why it is not decided.
