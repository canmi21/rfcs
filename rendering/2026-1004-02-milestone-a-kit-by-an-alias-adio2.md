---
name: 'Milestone A: Kit by an Alias'
title: 'An application written for Kit swaps one line of its package.json and is served by CTR, accepted by four measurements'
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

What is the first thing the framework can do, and what accepts it? Milestones are accepted by a measurement and not by a judgement -- seam's `spec/scope.md`, "Four milestones, and what accepts each".

## Decision

**An application written for Kit swaps one line of its `package.json` and is served by CTR.**
`"@sveltejs/kit": "npm:seamjs@<version>"` (seam's `spec/framework.md`, "The fork is the entry,
under the entry's name"), nothing else edited, and every page it serves is compiled -- wherever it
writes nothing CTR refuses. What CTR refuses is rendered by SSR -- the smallest component that can
be, at worst the route -- and named in the artifact's `coverage` (seam's `spec/together.md`); as A
was planned it was a compile-time error (seam's `spec/refusals.md`, "Every refusal is a
compile-time error"), which that supersedes.

Accepted when all four hold:

1. **Every response is Kit's, byte for byte, but where a difference is declared.** Each of Kit's
   test apps that builds, compared with `mise run compare` (seam's `spec/conformance.md`,
   "Stage 2"). **Met**: all eleven, `async`'s three streamed pages being the one declared
   difference.
2. **Kit's own specs pass through the fork, unedited**, every app's, in the build Kit's config
   runs them against, but for a failure a declared difference lists. **Met**: every spec of the
   eleven apps passes under the check build, but the declared one
   (seam's `spec/conformance.md`, "Stage 2").
3. **No request to a production build runs SSR where the artifact's `coverage` says `ctr`.** A
   route `mixed` or `ssr` runs it by declaration or degradation (seam's `spec/together.md`), and
   the check build exempts a route SSR whole. Held by a check build in which Kit's
   `root.svelte` throws whenever it is rendered, under which 1 and 2 still pass (`--kit-root=throw`,
   seam's `spec/conformance.md`, "Stage 2"). **Met**: every app compares the same and every
   spec passes with no request reaching Kit's root, error pages included
   (seam's `spec/framework.md`, "The error page").
4. **An application of the author's, moved.** `status`, held the way stage 2 holds Kit's apps, and
   no slower per request than through Kit (seam's `spec/conformance.md`, "Stage 3"). **The
   bytes are met, and the cost is Kit's within two percent**: sooner under load, about 1.3 ms
   later one request at a time.

`vite dev` is not in A: it is a target of its own, below, and it is met.

**A is the first release.** Once it is accepted the fork is published (seam's `spec/publish.md`).
**An application of the author's moves onto it once A is accepted and the dev server says what the
build will do** (below): an application served by CTR and developed where nothing says what CTR
makes of it is one whose author meets a shape CTR refuses only at the build, which makes every day
after the move worse than the day before it. The dev server says it now, while answering with
Kit's render. Until both hold, no application of the author's is changed -- `status` included, which
A measures from a copy. Then they move, a few small ones in the monorepo first and the rest as each
holds, so that what a migration meets is met in the author's projects before anyone else's. What it is then offered to others on is those migrations. `create-seamjs` changes with that release
and not before: it still writes the plugin's arrangement, `seam()` beside `sveltekit()`, and moves to
the alias when the entry is first published as the fork (seam's `spec/publish.md`, "`create-seamjs`").
