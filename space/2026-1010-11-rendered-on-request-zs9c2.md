---
name: 'Rendered on Request'
title: Every page of space is rendered in the Worker unless its route says it is prerendered, the choice made per route and never per app
status: landed
supersedes: []
blocked-by: []
commits: [web@cc9d5993]
references: []
---

## Question

Is a page of space rendered on each request in its Worker, or prerendered when it is built, and is
that chosen for an app or for a route?

## Options

- Prerender every page.
- Render every page on request.
- Render on request by default, and let a route whose content the build already knows prerender.
- Choose either one per app.

## Evidence

The console's short edge cache already does for a dynamic page what a static build does for a fixed
one, so rendering on request costs a page that does not change next to nothing. A part such as an
API tester may one day draw around a reader's credentials, which no static build can. A choice made
per app would lock an app out of whichever it did not pick.

## Decision

Rendered on request by default; a route whose content the build knows says `export const prerender =
true` and is served from the assets, with no Worker running. The choice is per route, never per app.
A response drawn around a reader is never cached.
