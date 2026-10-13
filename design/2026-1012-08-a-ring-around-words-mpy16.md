---
name: 'A Ring Around Words'
title: A ring on bare words stands one fixed room off their ink on every side, and its corner grows with the words
status: landed
supersedes: []
blocked-by: []
commits: [web@bebf97b6]
references: []
---

## Question

A ring drawn as keyboard focus is, flush on what it marks, sits well on a control, whose edge is
its own. Bare words have no edge: their box is their line, which leaves room above and below the
letters and none at their sides. How does a ring on words stand off them evenly?

## Options

- Flush on the words' box, as on a control.
- As much room either side as the line leaves above the capitals, the corner fixed.
- One fixed room off the ink on every side, the corner fixed.
- One fixed room off the ink on every side, the corner growing with the words.

## Evidence

On the Sizes page's interface card, a ring flush on a caption stood about 5px over its capitals and
touched its first and last letters, which read as cramped though the ring followed the rule. Pulled
to the width of its words first, a line was ringed where it is, and not across its column; the
cramped sides stayed.

Given the room its line leaves over the capitals either side, the ring stood evenly off a caption,
but the room grew with the line: about 4px at a caption and 10px at a display, so the larger the
words the looser the ring, which read as a box drawn around them rather than a mark on them.

With the room fixed and the corner fixed at 0.25rem, a ring around a display would turn as sharply
as one around a caption over a far longer side, so seen whole its corners read as tighter the
larger it is. A corner in proportion to the words turns as gently at every size.

Trimmed to `cap`, the ring still stood further over the capitals than at the baseline, 5.7px at a
caption and 8.7px at a display against 4px below: Chrome reads this sans's capitals as 0.86em tall
where they stand at 0.714em, the gap lib's `--font-sans-cap-height` already names, so the trim fell
short by the difference at every size. Given back from the trim, the ring stood 4px off the ink on
every side at all seven sizes.

## Decision

- **A ring on bare words is on their ink**: their box trimmed to their capitals above, and below to
  their baseline, or to their descent where a letter falls below it, `text-box: trim-both cap
alphabetic` or `cap text`.
- **It stands one fixed room off that ink on every side, `0.25rem`**, at every size, its top
  brought down by what Chrome's `cap` reads over the sans's real capitals, `1cap -
var(--font-sans-cap-height)`.
- **The ring is a mark that takes no room**, a pseudo-element set out from the ink, so the words
  stay where they are, and a top that must reach past the trim can.
- **Its corner grows with the words, `2/7em`**: 0.25rem at `body`, about 3.4px at `caption` and 9px
  at `display`.
- **The words keep their line; the ring is drawn on their ink**, the pressed element handing it down
  on `:focus-visible` as the kit's ring host does.
- **Each line is as wide as its words**, never stretched across its column, so the ring is too.
- **A control keeps its own edge**: a ring on a button or a pill is flush on its shape, as keyboard
  focus is.
