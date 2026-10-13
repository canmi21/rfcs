---
name: 'A Ring Centered on Capitals'
title: A ring on words with a letter below the baseline reaches as far up as down, so it stays centered on the capitals, and nothing beside it stands within its room
status: landed
supersedes: [z3y37]
blocked-by: []
commits: [web@3f3aa191]
references: []
---

## Question

RFC z3y37 keeps ringed words on their baseline and reaches the ring alone down past letters that
fall below it. Ringed so, does a word with such a letter still read as centered in its ring, and
what may stand beside a ring that takes no room?

## Options

- Reach down alone, as z3y37 does.
- Reach down and up by the same, so the ring is centered on the band from the capitals to the
  baseline.

## Evidence

The top bar's corner, `Design`, ringed 4px over its capitals and 7.4px under its baseline, read as
sitting low beside its mark, which is centered on the words: the ring's middle stood 1.7px under
theirs. Reaching up by the descender too, it read as centered, the `g` still inside it. The same
corner set its mark 2px from the name, so the ring, 4px out and 2px wide, met the mark; parted by
6px it cleared it.

## Decision

- **A word's box is trimmed to its capitals and its baseline, whatever its letters**, as z3y37 has
  it.
- **Where a letter falls below the baseline, the ring reaches down by the sans's descender,
  0.212em, and up by as much**, so it stays centered on the capitals and the letter stays inside it.
- **Nothing stands within a ring's room and width of words that can be ringed**, `0.25rem` and
  `0.125rem`, since the ring takes no room and would meet it.
- **The ring is 2px wherever it is drawn**, on a control or on words, as keyboard focus is.
