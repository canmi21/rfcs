---
name: "The Author's Entity Home"
title: The author has no entity home yet
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The author's structured-data identifier is `https://canmi.net/about#person`, chosen as the page
that will say first-hand who the author is -- see
web's `spec/architecture/entities.md`. That page does not exist, so the identifier
does not resolve. **`/about` is therefore a fixed address**: when it is built it is a
`ProfilePage` whose `mainEntity` is that person, the person's full node moves there, and every other
page keeps only the reference. Nothing else may take the path. The same page is where `article:author` and
`<link rel="author">` point once it exists.
