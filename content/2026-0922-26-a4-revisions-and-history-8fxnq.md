---
name: 'A4: Revisions and History'
title: 'Every save writes an immutable revision, a head per article, and its dates derived from them'
status: landed
supersedes: []
blocked-by: [rjw0s, 9adnf]
commits: [web@7b196dff, web@929c6f0d]
references: []
---

## Question

A text file under git has a history whether or not anybody asks for one; an article in the
collection has none until it is given one. A4 of web's milestones.

## Decision

- **Publishing writes a revision, and the chain is stored in reverse**: the newest carries the cid,
  older ones the patch back to the version before.
- **Publishing takes no text**: the draft row is the working copy, before and after.
- **Dates are derived from the revisions**, an imported article's one revision dated by its last
  modification.
