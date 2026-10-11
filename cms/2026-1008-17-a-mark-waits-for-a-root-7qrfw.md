---
name: A Mark Waits for a Root
title: A new mark waits for a root the corpus cannot be published under yet
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The root is one object written whole from the corpus on this machine, so publishing a mark
publishes every article beside it. On 2026-10-08 `console` was given marks of its own, their bytes
copied into the objects bucket by content id and the record changed in `data/record/symlinks.json`;
the dry run of `mise run publish web` then named them, and the bare `sitemap.xsl` registered since
the live root of 2026-10-01 -- and moved `markdown` and `views` on all six articles, since the
compiler has changed since and nothing has been published from it.

That is the gap the CMS's migration leaves open: the corpus is not yet the collection, and the
migration waits for the platform to be stable, which is the work in progress. So the root is not
republished, and a mark whose bytes are in the bucket answers at `/symlink/{scope}/{file}` only once
it is.

**What deciding it would cost.** Publishing the whole corpus now takes the articles onto the current
compiler before the migration settles what they are. A path that writes the marks alone into the
live root is a second writer of the one mutable object, outside the check that refuses a root naming
an object the tree does not hold -- platform's `spec/architecture/delivery.md`, "Every fixed name is
a record".
