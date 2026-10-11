---
name: "A Directory Row's Focus Ring"
title: A directory row asks for a focus ring and every rule that could draw one declines
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The license directory and the registry directory write their rows the same way:
`focus-ring-within` on the anchor, `focus-visible:outline-none` beside it, and a `focus-link-inner`
on the license name two levels down, inside the flex span that also holds the leader. Driven with
the keyboard at 1600px, no ring appears on either page. The row reports `outline-style: none` and
so does the name inside it, while every other control in the same tab order reports
`solid 2px oklch(0.623 0.214 259.815)`.

Three rules could have drawn it and none matches. The base layer's `:focus-visible` is overridden
by the row's own `outline-none`. `.focus-ring-within:has(:focus-visible)` wants the focused element
to be a descendant, and here the row is the focused element. `:focus-visible > .focus-link-inner`
wants a direct child, and the name is a grandchild. So the row is reaching for the container
variant of a vocabulary whose direct variant is the one it wants: its hit area and its visible
control are the same element.

Measured on the migrated license directory and on the registry directory, which is unmigrated and
writes the identical row -- both read the same, so this is not something the migration introduced.
Moving `outline-none` out of Tailwind's `utilities` layer and into StyleX's changes nothing here
either, because both of them beat the `base` layer the ring is declared in.

Deciding it is a choice between `focus-ring` on the row and lifting `focus-link-inner` to be a
direct child of it, and either changes what a keyboard user sees on two pages.
