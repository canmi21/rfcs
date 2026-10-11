---
name: 'Milestone B: CTR and SSR'
title: 'A component may be declared rendered by SSR and everything else is CTR, in Node, taken forward while A is worked'
status: accepted
supersedes: []
blocked-by: [adio2]
commits: []
references: []
---

## Question

Where does SSR enter a route, beside CTR?

## Decision

**Taken forward, the user's decision**, with two of its rules changed: the declaration is written,
and a refusal degrades to SSR rather than failing the build. seam's `spec/together.md` is the rule
as it now stands; what follows is B as it was planned, kept for its acceptance checks.

**A component may be declared rendered by SSR, and everything else is CTR.**

- **The declaration is per component.** The compiler reads every component's source and tree, so
  the component is the unit it can be asked at. Written `export const seam = 'ssr'` in its module
  script (seam's `spec/together.md`, "Declaring a component SSR").
- **It goes up and never down.** CTR is the lower layer and SSR the upper one. A CTR component's
  child may be declared SSR; a component declared SSR is SSR with everything under it, and nothing
  under it can be declared back to CTR -- Svelte's server render renders a component's children
  itself, so nothing is left there for CTR to take. A declaration that would step down is a
  compile-time error.
- _Superseded:_ **It is the author's, never the compiler's.** A refusal now degrades the component
  it is raised in, and the build names it (seam's `spec/together.md`).

Accepted when:

1. **An application declaring nothing passes A's checks unchanged.**
2. **A page with declared components is Kit's byte for byte**, held the way A holds a page: the
   same applications, with declarations placed, against Kit's build.
3. **Declaring nothing and degrading nothing ships nothing of SSR.** The server build of an
   application whose coverage is CTR whole contains no component compiled for Svelte's server,
   Kit's `root.svelte` among them, and of `svelte/server` only what CTR itself calls, which the
   check lists by name. Checked on the build's output, by a test, not by reading it.
4. **Declaring some ships SSR for those alone**: the declared components and what is under them.
