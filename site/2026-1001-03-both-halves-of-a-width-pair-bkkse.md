---
name: Both Halves of a Width Pair
title: The markdown target keeps both halves of a width pair
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

A `:t[...]{wide}` and its `:t[...]{narrow}` twin are one sentence drawn two ways, and the page shows
one. The compiler's markdown target writes both, so the homepage's view reads "I hope somedaySomeday
I hope" -- live at `/homepage.md`, 2026-10-01. Fixing it changes the published markdown, so it waits
for the corpus to be published again.
