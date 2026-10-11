---
name: Dev Answers With Kit
title: "vite dev answers with Kit's render, CTR checked behind each response, and the build held to the props it kept"
status: landed
supersedes: [5k5pj]
blocked-by: []
commits: [seam@5cabda59]
references: []
---

## Question

Under `vite dev`, CTR answered every request with the program, and a shape it refuses was Vite's error
page. A refusal in development stopped the author mid-edit over a page Kit would have rendered.

## Evidence

seam's `spec/build.md`, "The dev server answers with Kit's render, and CTR is checked behind it".

## Decision

The user's decision:

- **The dev server answers with Kit's render**, and CTR is compiled and checked behind each response,
  so what CTR makes of a page is said without being what is served.
- **The build is held to the props development kept**, each route's CTR checked against Svelte's
  render of them.
