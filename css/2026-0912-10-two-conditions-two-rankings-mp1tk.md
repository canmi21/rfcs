---
name: 'Two Conditions, Two Rankings'
title: Two conditions on one property are ranked differently by the two layers
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Both Tailwind and StyleX let a declaration be conditional, and where two conditions can be true at
once they disagree about which one wins. The newsletter's submit button is the case: `hover:opacity-85`
beside `disabled:opacity-60`, and a request in flight disables the button under a pointer that is
still resting on it, because Chrome matches `:hover` on a disabled control.

Tailwind emits its `hover:` block before its `disabled:` rule at equal specificity, so the dimmer
of the two wins -- measured on the unmigrated component, 0.6. StyleX sorts `:hover` after
`:disabled` and keeps doing so whichever order the pair is written in, and inside a
`@media (hover: hover)` query as well -- measured twice, 0.85. Neither ordering is documented and
neither vendor would notice changing it. The only reason this was caught is that the migration
drove the state; no snapshot has a disabled button in it.

The component states the exclusion instead, `:hover:not(:disabled)`, which makes the two mutually
exclusive rather than ranked and therefore reads the same under either ordering. What is left
unresolved is that this is a repair rediscovered per component. Nothing checks that a migrated pair
of overlapping conditions still resolves the way it did, and
web's `apps/site/scripts/css-layers.ts` is about the order of the layers rather than
about what sits inside one. Deciding it is either a rule that overlapping conditions are always
written as exclusions, or a check that can see the pair.
