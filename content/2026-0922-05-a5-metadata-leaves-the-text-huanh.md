---
name: 'A5: Metadata Leaves the Text'
title: "An article's path, description, publication, last-modified and its lock become rows, and the frontmatter empties"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The plan's milestones are agreed in order, each saying what it changes and what it cannot start before -- web's `spec/architecture/collection.md` holds the model they assume.

## Evidence

**A5's rows exist, and the one revision each imported article has says `lastmod`.** A chain of
one says both dates at once, and the text the collection holds is the one that was last edited --
so that is the honest value, and the publication date is what a single revision cannot also
carry. `at` is editable and lockable for exactly this: an article whose two dates should differ
gets a second revision or a correction, per article rather than by rule.

**A5 must not land before there is a surface that can edit what it moves.** The frontmatter is
editable in any text editor; a row in SQLite is not. So A5 ships with B4, or with a stopgap command
that sets a field. This is the one place in this plan where the obvious order blocks the author.

## Decision

- **Path, description, publication, last-modified and its lock become rows**; the frontmatter
  empties.
- **It ships with B4, or with a stopgap command that sets a field**, never before a surface can edit
  what it moves.
