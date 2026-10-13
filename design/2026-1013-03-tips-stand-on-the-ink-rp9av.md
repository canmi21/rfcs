---
name: 'Tips Stand on the Ink'
title: A tip points at what is drawn, a control's shape or words' capitals, and a row of tips is one band to the pointer
status: landed
supersedes: []
blocked-by: []
commits: [web@17f79489]
references: []
---

## Question

A tooltip sits a fixed gap above the box it is wrapped round. On space's pages that box was not
always what the eye takes for the thing named: on words it was their line, and on a word set apart
by a margin it was the margin too. And the rule that marks with tips in a row share the gaps between
them, written in lib's spec/design/components.md, was kept by the swatch alone. What does a tip
measure to, and how does every row keep the shared gaps?

## Options

- Measure to the box the tip wraps, whatever it holds, as the tooltip does by itself.
- Measure to the ring's box: the shape for a control, the ink and its room for words.
- Measure to what is drawn: the shape for a control, the capitals for words.
- Keep each row's reach in the component that draws the row.
- Give every row one reach to share, beside the ring.

## Evidence

Measured to the box, the gap above a tip's mark read from 9 to 28 pixels across one card: words
carried their line's leading over their capitals, more at every size up the ladder, and a caption
under a name carried the margin that set it apart, inside the box the tip measured. Measured to the
ring's box, every tip stood 9 pixels off it, and words still read as tipped higher than the button
beside them, by the room the ring keeps off their ink. Measured to their capitals, a word's tip
stands as far off its letters as a button's off its edge, which is how both are read.

Making a word's box its capitals' without moving it took a line around the tip, in the word's own
size, so the line it set is kept by the line's own strut, and the tip's box inside it shrunk to the
capitals, which always fit within that line. A first try set the line by margins worked out from the
sans's ascent and descent, and missed by up to half a pixel where Chrome rounds the leading to the
screen's pixels.

The shared gaps were written down and kept once, in the swatch; every row drawn since -- the type
ladder, home's three cards, the focus page's rows -- had none, and the pointer crossing them fell
into every gap and lost the tip. A rule kept in one component reaches only that component.

## Decision

- **A tip points at what is drawn**: a control's shape, and words' capitals, 9 pixels off either.
- **Words with a tip are held in a line of their own**, in their size, around the tip: `line`,
  `fit` and `fitted` in `@canmi/space-ui`'s `ring.ts`.
- **Every row of tips takes one reach**, `reach` in the same file: each mark's pointer area out into
  half of each gap beside it, and up or down to the row's height where it is shorter, words' own
  starting at their ring's box. A row states `--reach`, half its gap; the swatch takes it too.
