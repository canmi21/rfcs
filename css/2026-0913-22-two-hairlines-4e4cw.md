---
name: Two Hairlines
title: 'Two named surfaces disagree about what a hairline is, and each is internally consistent'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The entry on one border and two spellings counts `borderWidth` as `1px` ten times and `0.0625rem`
seven, and reads the split as a scale nobody named. Naming the surfaces says something the count
could not: the split is not scattered. Every one of the eight sites of
`surfaces.paper` in `@canmi/kit` writes `1px` -- the three block frames, the
menu, the popover, the modal, the search panel and the newsletter's pill -- and every one of the
three sites of `surfaces.interactive` writes `0.0625rem` -- the repository card, the tweet card and
the support pill. Neither group has an exception.

So one disagreement now sits between two names rather than across seventeen declarations, which is
better and worse at once. Better because it is one decision in one file affecting two lines. Worse
because a name is read as a decision, and `surfaces.interactive` asserts that a border which
answers a pointer is a rem hairline -- which nobody chose. It is what the two cards and the pill
happened to carry, and the recipe is a description of the site rather than a ruling about it.

**The consequence for anyone reading the two together is the part worth recording.** Side by side
in one file they look like one surface and its hover states, and they cannot be: composing
`interactive` onto `paper` would set the border twice from two spellings and the second would win
silently. That is not a defect in either group, it is the reason they are two groups, and the
module comment says so -- but it says so in a comment rather than in anything that would stop the
composition being written.

Two whole-box borders belong to neither group and are named here so a later count does not read
them as members. The cargo and tokei tooltips draw a `var(--color-paper)` ground with the
`0.0625rem` hairline, so they are the one combination that crosses the two answers; they are also
the verbatim copy the entry on the shared vocabulary already holds. `mermaid.node` and
`quadrant.box` take the rem hairline over a different ground.

Deciding it is choosing one spelling and moving between three and eight declarations onto it, which
changes what a reader who has enlarged text sees on every surface that moves, so it wants the gate
rather than an argument. What it cannot stay as is two names that each answer the question
confidently and differently.
