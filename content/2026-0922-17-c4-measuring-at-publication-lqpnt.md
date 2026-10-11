---
name: 'C4: Measuring at Publication'
title: 'A controlled headless browser answers at publication what only a browser knows, and the answer is stored'
status: accepted
supersedes: []
blocked-by: [ga5oq]
commits: []
references: []
---

## Question

The plan's milestones are agreed in order, each saying what it changes and what it cannot start before -- web's `spec/architecture/collection.md` holds the model they assume.

## Evidence

C4 is where the wrap policy, RFC p4r83, is answered, and it generalizes: a paragraph's
line breaking, a diagram's layout and anything else whose answer exists only inside a real browser
is decided once, under a fixed column and a known font, and written into the published object. A
measurement taken in whatever browser happens to be editing would record that device instead.

## Decision

- **A controlled headless browser answers what only a browser knows, and the answer is stored.**
