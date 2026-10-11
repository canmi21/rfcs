---
name: 'One Border, Two Spellings'
title: 'One border, two spellings, and the migration is what put them side by side'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Counted inside the thirty-five `stylex.create` blocks the site now carries, `borderWidth` appears
as `'1px'` ten times and as `'0.0625rem'` seven. Both are one pixel at the default root size and
neither is at any other, so the site has two answers to what a hairline is and they part company
the moment a reader enlarges text.

Two files carry both. In web's `libs/prose/src/blocks/quadrant.svelte`
and web's `libs/prose/src/blocks/mermaid/mermaid.svelte` the
pre-migration source declares only `0.0625rem`, in a scoped rule; the `1px` arrived from Tailwind's
`border` utility in the same component's markup. So neither spelling was invented here and the
migration is faithful in both directions -- it is the act of putting the layout layer's answer and
the selector layer's answer into one object that makes the disagreement legible. It was always
there and nothing could see it.

`fontFamily` divides the same way and the halves are genuinely different. Fifteen declarations say
`var(--font-mono)`, which is Tailwind's theme variable and is defined nowhere in this repository;
four say the literal `ui-monospace, SFMono-Regular, Menlo, Consolas, monospace`. Tailwind's stack
carries Monaco, Liberation Mono and Courier New; the literal one does not. A reader on a machine
that has Monaco and not Menlo reads two different fonts on one page today, and has for as long as
both spellings have existed. web's `libs/prose/src/blocks/github.svelte`
says in a comment why it kept the literal one, which is the correct behavior for a migration and
also the reason this entry exists rather than a commit.

The rest is a scale nobody named, and the counts say which parts are already agreed and which are
not. Color is agreed: of 198 color declarations, 186 read a token and the twelve that do not are
keywords -- `transparent`, `inherit`, `currentColor` -- plus two literal fills inside one icon.
Weight is nearly agreed at three values for 32 declarations. Radius is not: 42 declarations take
eleven distinct values, and two of them, `calc(infinity * 1px)` and `624.9375rem`, are the same
pill written twice. Size is not: 90 declarations take twelve values including `0.71875rem` and
`0.78125rem`, eleven and a half and twelve and a half pixels, which are two components each
rounding a judgment rather than a decision anyone made twice, and one `0.9em` among rems. Line
height is the least settled of all -- 41 declarations, fifteen values, mixing unitless ratios with
`rem` lengths, which are not the same kind of thing.

The pill is worse than that sentence says, and the extra spellings are outside the count. Those 42
declarations are the visual layer's, and a corner that is fully round is written four ways across
the site: `calc(infinity * 1px)`, `624.9375rem`, `9999px` -- in
web's `libs/prose/src/components/preview.svelte`'s scoped block and again on
`.article-preview-thumbnail [data-icon-bar]` in
`@canmi/ui/primitives` -- and `50%` on
web's `libs/prose/src/blocks/github.svelte`'s language dot. The last is the
one that is not a synonym: a percentage is a share of the box, so on anything that is not square it
draws an ellipse where the other three draw a stadium. Every element carrying it is square today,
which is why nothing looks wrong and why nothing would say so if one stopped being.

Deciding it is not one choice but three, and only the first is cheap. Naming the scale that exists
costs nothing and changes nothing. Collapsing `1px` and `0.0625rem` into one name is a visual
change on whichever side loses, so it needs the gate run over it rather than an argument. And the
two mono stacks are a question about which one is right, which is not a layering question at all.
