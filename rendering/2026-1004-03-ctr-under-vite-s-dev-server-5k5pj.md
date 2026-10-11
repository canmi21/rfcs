---
name: "CTR Under Vite's Dev Server"
title: "vite dev is taken by CTR as a compile target of its own, after A, gating the author's applications moving"
status: landed
supersedes: []
blocked-by: []
commits: [seam@ae697f09, seam@68a3ee05, seam@31e17589]
references: []
---

## Question

Under the dev server Svelte compiles with `dev` and `hmr`, and its bytes are another set. Is `vite dev` CTR's too, and when?

## Decision

**`vite dev` is taken by CTR too, as a compile target of its own.** It is after A, and gates the
author's applications moving, though neither A nor the release; before it, the dev server rendered
by SSR as Kit's does and said nothing of CTR, and a shape CTR refuses was met at the build. A target of its own because the bytes are another set:
under the dev server Svelte compiles with `dev` and `hmr`, its server output differs from a
production build's -- a `<!---->` after every component that `clean_nodes` would leave alone
(seam's `spec/framework.md`, "The comparison that counts") -- and the dev client hydrates those.
So it has its own measurement, Svelte's samples rendered under those options and Kit's apps under
`vite dev` against Kit's dev server, `dev-only` among them and the dev half of each app's
Playwright config. And the compile becomes incremental: a route compiled when it is first asked
for and compiled again when a file its components reach changes, read off Vite's module graph.

**Met, and then changed.** The dev server now answers with Kit's render and checks CTR behind it,
the user's decision (seam's `spec/build.md`, "The dev server answers with Kit's render, and CTR is
checked behind it"); what follows is what was met while it answered with the program. Kit's
twelve apps under `vite dev` answer every page as Kit's dev server does
(seam's `spec/conformance.md`, "Under the dev server"), the plugin's sample does under an edit
of a component, of a server `load` and into a shape CTR refuses (`pkgs/plugin/src/dev.test.ts`), and
`status` does, its first compile 2.1 s and an edit reaching the next request in 1.7 s where Kit's
takes 0.5 s. Two decisions of the user's shaped it then -- a refusal was Vite's error page rather
than a fall back to SSR, since superseded by seam's `spec/together.md`, and the script Svelte's `dev` writes about a misplaced element is reported by the
compile and not written. Svelte's own samples under the dev options are not yet measured; the
suite renders under the production runtime, and a run of it under the development one is owed.
