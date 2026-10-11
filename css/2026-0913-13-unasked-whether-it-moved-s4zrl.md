---
name: Unasked Whether It Moved
title: Nothing in the tree asks whether a declaration moved
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `spec/architecture/css/extraction.md` now says what the gate on a naming change is -- build,
and compare the multiset of emitted declarations per layer -- and no check makes that comparison.
It was two scripts written for this change and kept out of the tree with the migration's own
harness, so the next person to name a value has to write them again or trust a reading.

The comparison would be cheap and it is not the migration's snapshot harness: one build, which
`check-css` already runs, and a sorted diff of what the stylesheet declares against a committed
list. What it costs is a file that has to be regenerated on purpose whenever a value is meant to
change, which is every other kind of edit to the visual layer, and a stale one fails loudly on
work that is correct. Whether that trade is worth making is the decision.

One consequence of the mechanism has nowhere to be recorded but here, because it is a fact about
this repository rather than about StyleX. **A constant's key spelling is part of the stylesheet.**
The class is hashed from `var(--<consthash>)` and the hash is over the module path and the key, so
renaming `text.px14` or moving the module rewrites rules while changing no declaration. Every name
in vocabulary.stylex.ts in `@canmi/kit` was therefore argued once,
before anything read it, and a later rename is not the free edit it looks like.
