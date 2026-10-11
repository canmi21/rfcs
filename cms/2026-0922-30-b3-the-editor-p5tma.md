---
name: 'B3: The Editor'
title: "A web client against local writes, previews in the site's own components, and creates with a rid"
status: landed
supersedes: []
blocked-by: [9adnf, 8fxnq, ex27b]
commits: [web@e98a79c1, web@09ccf58e, web@3562e723, web@72f2156f]
references: []
---

## Question

With `local` answering HTTP and articles in the collection, what does the author write in? B3 of
web's milestones.

## Decision

- **An editor, against `local`, on its own pinned port.**
- **Create gets a rid**, reserved only once the new article is named.
- **The document is the text**: a CodeMirror editor drawing marks and blocks from the site's own tree
  and components, the ProseMirror one removed.
- **Unsaved keystrokes are the browser's**, in `localStorage` keyed by rid, until a save writes the
  draft row.
