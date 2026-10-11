---
name: The Homepage Lists Everything
title: 'The homepage lists every article, and will not be able to for long'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`+page.svelte` renders one card per article the API answers with, and the API answers with all of
them. Six today. The intent is a fold -- five, or thereabouts -- and a route carrying the rest,
and none of that is decided: whether the fold is a count or a date, whether the full list is
paginated or one page, whether it has its own card and its own place in the sitemap, and what a
reader on a phone sees instead of a hover.

It is written down here because two things already built assume the list is short and will quietly
stop being right. The homepage warms every article it lists on a device with no pointer, which is
sound for six and wasteful for sixty; and the read-count batcher carries up to 24 keys, which is a
number chosen against today's list rather than against anything. Both follow whatever the fold
turns out to be, so neither is worth changing before it is decided.
