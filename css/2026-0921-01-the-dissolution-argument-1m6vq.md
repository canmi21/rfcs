---
name: The Dissolution Argument
title: "The dissolution section's argument stopped following from its premise"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `spec/architecture/css/layers.md`, "`utilities.css` is dissolved, not given
a position", settles the question this way: **every rule in it belongs to a layer that already
exists, so the file has nothing left to be.** That inference held while "a layer that already
exists" meant a layer somewhere other than this file. "The frame is a stack, and a declaration
written to lose goes low in it", added later to the same file, makes `base` and `components` two
positions this file itself opens, and records that it already writes into both. The premise stops
entailing the conclusion, and the section's own children confirm it: the `.focus-ring` family
belongs to an existing layer and stays in it.

**What gives is the word "so", not necessarily the conclusion.** Three grounds the axis never
supplied are still open, and dissolving the file would need one of them: the file is a grab-bag of
four unrelated things, which is a filing objection rather than a layering one; `base` and
`components` can be opened from any stylesheet, so nothing about those positions requires this
one; and the same file's test for a fourth layer -- "A layer is a position in the cascade.
Something that does not need a new position is asking for a new file, and a file is not a layer"
-- says the number of files is not a layering question in either direction.

**Where each group goes has since been ruled, and none of the rulings is the inference above.**
Checked against the tree on 2026-09-21:

- `.article-rail`, `.article-column`, `.meta-language` and the `:lang()` prose rules on
  `:is(.article-content, .article-summary)` stay in `@layer components`, by
  web's `spec/architecture/css/layers.md`, "A frame declaration the markup cannot
  show stays in a frame stylesheet". They are the frame and they are already in it, so there is
  nothing to move.
- `focus-link`, `spring-underline`, `article-link` and `jump-target` stay hand-written recipes,
  by that file's "A layer is not a mechanism", condition (a). All four are written by
  web's `libs/compile/src/compile.ts`, which cannot reach StyleX.
- `.selectable` is gone rather than moved: no rule and no call site is left anywhere in the tree.
- The `.focus-input` family is in the escape hatch in
  web's `apps/site/src/lib/newsletter/newsletter.svelte`, and no `focus-input` rule
  remains in `utilities.css`.
- The focus families and the interface classes are decided for a layer of their own and the move
  has not landed. Read here, they are still in `utilities.css`'s and `app.css`'s
  `@layer components` -- which is the state observed, not the disposition taken.

**What is not answered is whether the file empties at all**, and that is the whole of what is left
of this entry. With those groups staying and the `:root` rail tokens going to
@canmi/kit/tokens under the entry below, what remains in `utilities.css` is a question
for whoever picks it up rather than a backlog item with a known answer. The count that was here --
26 rule blocks, 18 unable to move on the rules as they stood and 2 awaiting a decision -- predates
every ruling above and is evidence of nothing now; counting again is the first thing that work
does. Moving what is left to `app.css` under the same `@layer` wrappers would be a rename rather
than a dissolution: `app.css` is itself one `@layer components` block now, wrapped on the reason
`utilities.css` records at its own wrapper, so `.pill` and `.value` sit at the position the rules
would arrive at.
