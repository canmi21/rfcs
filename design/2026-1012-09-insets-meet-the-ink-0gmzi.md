---
name: 'Insets Meet the Ink'
title: A box's inset is measured to what is drawn in it, words to their capitals and shapes to their edges, so it reads the same on every side
status: landed
supersedes: []
blocked-by: []
commits: [web@bebf97b6]
references: []
---

## Question

A box set in by one length on every side reads even only where what stands at each side starts at
its own edge. Words do not: their line leaves room over their capitals, which adds to the inset at
the top while a button at the foot and the words at either side meet it exactly. What is a box's
inset measured to?

## Options

- To the boxes of what it holds, one padding on every side.
- To what is drawn: the room over a first line's capitals taken off the top, shapes met at their
  edges.

## Evidence

The interface card on the Sizes page, padded 1.25rem all round, stood 20px off its words at the
sides, its pill at the right and its buttons at the foot, and about 26px off the capitals of its
first line, which read as a heavier top. A card on the design pages already trims its title to its
capitals for the same reason, so its inset is its sides'.

## Decision

- **An inset is measured to what is drawn**: words to their capitals, a shape to its edge.
- **The top gives back the room its first line leaves over the capitals**, `(1lh -
  var(--font-sans-cap-height)) / 2` in that line's own size, the sans's real capitals rather than
  Chrome's `cap`, which reads them too tall, so the capitals stand as far in as the words at the sides and the shapes
  at the foot. A box set in a size of its own for this is set in its first line's.
- **A shape beside that line stands out of it as it must**, centered on it, rather than making the
  line taller, so a pill by a name rises a little over the capitals.
- Where the line's box can be trimmed instead, `text-box: trim-start cap alphabetic` does the same
  and is used, as a card's title is; a line that is pressed or ringed keeps its box, so its room is
  given back from the inset.
