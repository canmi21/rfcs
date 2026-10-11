---
name: Two Colors Outside the Tokens
title: "Two of the site's colors are not the token layer's, and cannot be read from it"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `libs/prose/src/blocks/link-card.svelte` writes its title and its corner
arrow in `text-black` or `text-white` according to the tone the block declares. Measured across the
markup, those two elements are the only users of either utility on the site, and neither color is
in `@canmi/kit/theme`: they are Tailwind's own `--color-black` and
`--color-white`.

web's `spec/architecture/css/authoring.md` says a color is read as the variable the token layer
declares and is never retyped, and neither half of that is available here. Retyping gives `#000`,
which the rule forbids and which puts a color somewhere other than `@canmi/kit/tokens`. Reading
`var(--color-black)` works only for as long as some utility still names it: `app.css` imports
`tailwindcss` rather than declaring `@theme static`, so a theme variable no generated utility
mentions is not emitted at all -- and moving the last two uses out of the markup is what stops it
being mentioned. The declaration would resolve to nothing and the caption would fall back to
whatever the cover inherits.

So both stayed in the markup and the card is the one migrated component whose colors are not all
in one place. Deciding it is either a black and a white in `@canmi/kit/tokens`, which is a decision about
the palette rather than about this card -- a caption over a photograph is the only thing on the site
that is deliberately the same color in both themes -- or a rule that Tailwind's own theme variables
are readable from the visual layer, which needs a way to keep them emitted.

That last half is the shadow entry above arriving from the other direction. There the visual layer
cannot make an `@property` registration; here it cannot keep a `@theme` variable alive. Both are a
value the visual layer can state only while the markup somewhere else keeps a class.
