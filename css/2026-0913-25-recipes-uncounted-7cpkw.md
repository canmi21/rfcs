---
name: Recipes Uncounted
title: The vocabulary counts components and a recipe is not one
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `spec/architecture/css/extraction.md` admits a value to the vocabulary at three components.
`0.125rem` as a `border-radius` is written in two of them --
web's `libs/prose/src/blocks/cargo/cargo.svelte` and
web's `libs/prose/src/blocks/tokei/tokei.svelte`, the second twice -- and now in
`surfaces.quietControl` in `@canmi/kit` as well, which is a third file and not
a third component. It is Tailwind's `--radius-xs`, the one step of that scale
`vocabulary.stylex.ts` in `@canmi/kit` does not name, and the reason
it does not is that it was below the bar on the day the scale was written.

Whether a recipe counts toward the bar is the question, and it is not the same question as whether
this particular value should be named. A recipe is where a value goes to be written once, so
counting it as a site lets one extraction push a value over a threshold whose whole purpose is to
count how many places arrived at it independently. Counting it as nothing leaves a value sitting
in the vocabulary's own neighbourhood as a literal. The answer applies to every recipe written
after this one, which is why it is here rather than settled in passing by whoever writes the next.
