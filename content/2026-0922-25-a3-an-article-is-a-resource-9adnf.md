---
name: 'A3: An Article Is a Resource'
title: 'An article is a typed resource, granted from the same space and the same register as the rest'
status: landed
supersedes: []
blocked-by: [rjw0s]
commits: [web@d455930a, web@66f0a4d4]
references: []
---

## Question

Articles were text files named by their paths, outside the identities every other resource is
granted. A3 of web's milestones.

## Decision

- **Articles become `document.article`**, granted from the same space and the same register.
- **A draft is a reserved id, a body and a bag of optional metadata**: a resource may be typeless while
  its id is reserved.
- **The corpus enters the collection**, each document given an address, a content and a first
  revision; a draft nobody has typed takes a row and no type.
