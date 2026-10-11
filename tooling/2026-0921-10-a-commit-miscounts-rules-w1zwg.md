---
name: A Commit Miscounts Rules
title: A commit subject says sixteen rules where twelve were moved
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`d662919f`, "refactor: sixteen ordinary scoped rules become utilities and surfaces", is wrong in
its subject and right in its body. Twelve rules were acted on, not sixteen; the body's figures --
twenty-six declarations, eighteen StyleX keys, thirteen markup tokens -- are machine-checked and
stand. The census reads 33 ordinary rules before and 24 after, because three of the twelve were
split rather than moved: `github`'s and `twitter`'s `.corner` still hold `right`/`bottom` and
`dial`'s `.face` still holds `place-items`, all three unruled by the enumeration.

The arithmetic that makes 33 whole, since a later reader will try: 12 moved, 13 keyframe-bound,
1 `.focus-input` held by its own section, and 7 held by rules already written down -- the code
block's three by migration.md's set rule, tokei's two by this file, `support`'s `.reveal-mask` by
that same set rule, and `footnotes`' `.notes-fold:not(...)` whose only declaration is an unruled
`mask-image`.

**Declarations are the honest unit here**, because a rule that splits is neither moved nor kept.
