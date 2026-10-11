---
name: A Clip Resolved Into Bytes
title: A clip is the one resource reference the compiler still resolves into bytes
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

platform's `spec/architecture/resource.md`, "A rid is resolved three times", says a
compile may bake the article's own shape and nothing derived from a resource's current content.
Two of the three references that name a resource now obey it: `::linkcard`'s mark and `::image`'s
picture compile to a rid under `resources` and are resolved per render. `::video` does not. Its
block carries `src` as a rid and then, beside it, `rungs` with absolute CDN URLs, `poster` as
another, `preview`, `width`, `height`, `gain` and `captions` -- every one of them a fact about what
the clip currently holds. Measured on `hindsight/except-me`: 15 image blocks contribute 15 rids to
`namedResources` and its 3 video blocks contribute none, because a `video` block has no `resources`
key for the collector to read.

The consequence is the one that paragraph exists to name: re-encoding a clip rewrites the `rungs`
inside every content object that plays it, so the object's id moves and the article is republished
for a change the article did not make. It is also the reason an editor that runs online cannot yet
write a clip reference -- the write would need a recompile to take effect.

**What deciding it would cost.** The record side is ready: `media.video.clip` is in the catalog
with its rungs and caption tracks, and `media.image.frame` already points at its clip through
`source`. What is not settled is which of a clip's several references are roles on one block --
the clip and its poster frame are two resources, and `namedResources` reads a map, so
`{ clip, poster }` is the shape it is waiting for -- and whether the player's fallback for a
reference nothing has imported survives the move, since today that fallback is "the resolved
fields are absent" and after it would be "the record is absent". Both are decisions about the
player rather than about the resolution path, which is why the two loops that built that path
stopped at the edge of them.
