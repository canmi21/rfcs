---
name: 'A Neutral Ground for Both Themes'
title: Where a specimen draws both themes and a value needs its theme's ground, its box stands on one gray the two share, CIELAB L* 50
status: landed
supersedes: []
blocked-by: []
commits: [lib@9e745400, web@c9c3a9a0]
references: []
---

## Question

A specimen is drawn in the page's theme. When are both themes drawn at once, and when they are, what
does the box holding them stand on, so neither reads as the odd one out?

## Options

- **When**: wherever a value differs by theme; or only where the rule is why the two differ, and on
  the page about light and dark.
- **Ground**: the page's own base; white or black; a gray chosen per theme; or one gray the same in
  both themes, a new token outside the step rule.
- **Reach**: every specimen box; or only those where a value needs its theme's ground to be read,
  and those whose values run to one end.

## Evidence

A page drawn twice, in both themes everywhere, was hard on a reader in either; light and dark need
no case made for them except where the rule itself is the difference, as a hue's pair is.

On the page's base, the window of the reader's own theme melted into the page and the other stood
out as a patch, whichever theme the reader was in. A gray halfway between them, the same in both
themes, left neither window at home.

A family of every gray, each a bare value whole on its own, read worse on the neutral than on the
page's base, and went back to the base. A stretch from black to L\* 20 on the dark page's base lost
its first cells into it; on the neutral each cell stood apart. A white box for that stretch was
tried, and went back to the neutral.

## Decision

- **`--background-neutral`, `oklch(0.569 0 0)`, CIELAB L\* 50**, one value in both themes, outside
  the 1.5 step rule, held to both by mono's test.
- **A specimen is drawn in the page's theme**, both themes side by side only where the rule is why
  the two differ, and on the page about light and dark.
- **A box stands on the neutral where both themes are drawn and a value needs its theme's ground to
  be read**, what names each window written inside it in its theme, **and where values run to one
  end**, near black or near white. A bare value keeps the page's ground.
- **It is a specimen box's alone**, never a page's or a component's.
