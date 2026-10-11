---
name: The Three-Component Threshold
title: 'The three-component threshold is a memory, and nothing counts the components'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

**Since answered: `check-css-extraction` counts the components behind every name** -- see
web's `spec/architecture/css/procedure.md`. What follows is the finding as it
was, kept for why the gate exists.

web's `spec/architecture/css/extraction.md` admits a value to the vocabulary at
three components, and a declaration group on the same bar plus one more. Nothing counts them. Every
admission and every refusal so far is a number somebody held in their head at the moment they wrote
the value, and whoever reads the result inherits the conclusion without the count.

"The extraction threshold counts one layer and the vocabulary lives in three" above is a different
finding about the same number: that one is about which layers a count should read, and this one is
that no count exists to point at a layer. A `check-layers` reporting application sites per value
would make the threshold a measurement rather than a memory, and the two questions only separate
once something is doing the counting.

Measured 2026-09-21 by reading `mise.toml` and `apps/site/scripts/`: `check-css` builds the site and
runs `css-layers.ts`, which reads the built stylesheets and holds the order of the three layers. No
task in this repository reads a declaration's value and counts where else it is written. Deciding
this costs a parser over the site's TypeScript, and a threshold that can fail a build is a number
that has to be right on the days nobody is thinking about it.
