---
name: "An Avatar's Placeholder"
title: 'An avatar has no placeholder a page can paint before it arrives, and what an account holds for one is open'
status: draft
supersedes: []
blocked-by: [nzscl]
commits: []
references: []
---

## Question

The console's sidebar shows the signed-in account's avatar, and until accounts exist it is the
author's, fetched from GitHub through the CDN's proxy -- web's `spec/architecture/console.md`. The
`<img>` is in the server's HTML; the picture is a second request, so for a moment there is an empty
circle, and the console paints a neutral one under it in the meantime.

The resource layer already answers this for every other picture: a record carries
`image.thumbhash` and its decoded copy, and a page rendered on the server inlines the placeholder for
every rid it names -- platform's `spec/architecture/resource.md`, "A rid is resolved three times,
and each stage bakes only what it can know".

## Evidence

Once accounts exist, an avatar a person uploads or links is a resource like any picture: its bytes in
the platform's store, its record holding the thumbhash taken at import, so every consumer -- the
console, a comment, a page that names the author -- renders it on the server with its placeholder
already painted. What is open is whether an account holds a rid or an avatar of its own kind, and
whether an avatar linked from elsewhere, GitHub's today, is imported once or followed and
re-imported when it changes.
