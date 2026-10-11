---
name: The Resolution Path Untested
title: "The resolution path on the site has no tests, because the site's modules do not resolve under vitest"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`apps/site/src/lib/published/` is where a rid becomes a record: the batch question, the split at
what one request carries, the memo, and the two cache windows that decide what is stored after a
miss and after an outage. Nothing in it is covered. Every test under `apps/site` imports by
relative path and none touches `$lib` or `$app`, and that is not a convention -- the root
`vitest.config.ts` declares no alias for either, so a test that imported `$lib/published` would
fail to resolve before an assertion ran.

The behavior was verified by execution rather than by assertion while this was written: driving
`publishedResources` against a recording fetch under a throwaway config shows one question for a
page, a rid the corpus does not publish stored and not asked for again, nothing stored at all when
the API cannot be reached, and a 65-rid page split into 64 and 1 rather than refused. Each of those
was confirmed to fail when the behavior behind it was removed. None of it is committed.

**What deciding it would cost.** Three aliases -- `$app/environment`, `$lib`, and a stub for
`@tanstack/svelte-query`, which publishes `.svelte` source that Node cannot load. The first two are
a line each. The third is the decision: either a stub module that every test in the workspace would
then resolve to, or `@sveltejs/vite-plugin-svelte` in the root config, which pulls the site's whole
Svelte pipeline into the root test run as well. Neither is a change to make as a side effect of
a task about batching.
