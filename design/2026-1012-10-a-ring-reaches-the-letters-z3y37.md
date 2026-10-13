---
name: 'A Ring Reaches the Letters'
title: Words ringed on their ink keep a box that ends on the baseline, and only the ring reaches down past letters that fall below it
status: landed
supersedes: []
blocked-by: []
commits: [web@118c5a64]
references: []
---

## Question

RFC mpy16 trims words with a letter below the baseline to their descent, `text-box: trim-both cap
text`, so the ring takes in the tail of a `g` or a `p`. Where such words stand in a row with words
that have none, does the ring still stand one room off every letter, and do the words still line up?

## Options

- Trim to the descent, as mpy16 does.
- Trim every word to its baseline, and reach the ring alone down to the letters that fall, by this
  sans's own descender.

## Evidence

Measured on this sans, `g` falls 0.212em below the baseline and `p`, `y` and `j` 0.198em, at every
weight, while the font's descent, which `text-box` trims to, is 0.34em: the ring stood about 0.13em
further under such words than over them. In the top bar, where each layer's words are centered in
their link, Components and Layouts, trimmed to that descent, stood 2.5px higher than Foundations and
Primitives. Trimmed to the baseline with the ring reaching down by 0.212em, all four shared one
baseline and the ring stood 4px under a `g`.

## Decision

- **A word's box is trimmed to its capitals and its baseline, whatever its letters**, so it lays out
  as any other word does.
- **Where a letter falls below the baseline, only the ring reaches down**, by the sans's descender,
  0.212em, so it stands one room under that letter.
- **The descender is named beside the code that reads it** until the style holds it next to
  `--font-sans-cap-height`.
