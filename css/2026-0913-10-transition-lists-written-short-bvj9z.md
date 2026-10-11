---
name: Transition Lists Written Short
title: 'A `transition` shorthand sets five lists and the migrated form writes three'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`transition: background-color 150ms ease-out, border-color 150ms ease-out` on
web's `libs/prose/src/blocks/github.svelte`'s repository card is one declaration and
five computed longhands, each of them a two-item list: `transition-property`, `-duration`,
`-timing-function`, `-delay` and `-behavior`. The shorthand sets the last two to their initial
values once per item, so the element computes `transition-delay: 0s, 0s` and `transition-behavior:
normal, normal`.

The migrated form writes three of the five. It was arrived at on
web's `libs/prose/src/blocks/code-block.svelte`'s `copyIcon`, whose comment says
exactly why the curve is stated twice -- "a transition's other lists are read per property, and one
value against two properties is not the same computed style as two" -- and then stops at the curve.
`transition-delay` and `transition-behavior` are left to their initial values, which are
single-item, so a two-property transition that moves into the visual layer computes `0s` where it
computed `0s, 0s` and `normal` where it computed `normal, normal`.

Nothing renders differently and nothing animates differently: the delay is zero either way and the
behavior is normal either way. What changes is the computed value, and
web's `spec/architecture/css/migration.md` makes the computed value the measure -- it is the same
argument that keeps three `--tw-gradient-*` variables in every migrated `transition-colors`, where
the names are another framework's and the values animate nothing. The two answers disagree, and
they disagree inside one property.

Whether the gate would catch it is not known here. The harness is not in the tree, and
web's `spec/architecture/css/migration.md` says only that the list has gone from seventy-three
properties to ninety-eight and that a migration moving a property nobody has compared before should
say so. This is that: `transition-behavior` reached Chrome in 117 and is younger than most of the
list.

It reaches only the transitions that were written as a shorthand naming more than one property.
A single-property shorthand sets a one-item delay, which is what the initial value already is, and
a Tailwind utility such as `transition-transform` writes one duration against its four properties
itself -- which is why `code-block.svelte`'s `chevron` is right as it stands and its `copyIcon`,
moved from a two-property shorthand, is the first instance. `github.svelte`'s card is the second.

Deciding it is either that a moved shorthand states every list it set, which makes the visual layer
wordier at each site, or that the two lists whose values are inert are outside what sameness means
-- which needs writing down somewhere a reader will find it before they write the third one.
