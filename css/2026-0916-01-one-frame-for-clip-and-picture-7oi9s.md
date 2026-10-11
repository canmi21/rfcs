---
name: One Frame for Clip and Picture
title: "Whether a clip and a picture should draw one frame is a question about `blockFrame`'s users"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `spec/architecture/css/extraction.md` records why a video clip takes `picture.svelte`'s 2px
edge and 1rem corner rather than `surfaces.blockFrame` in `@canmi/kit`'s
hairline and `radius.xl`: its neighbor in a column of prose is almost always a picture, and two
different corners side by side would read as a mistake. Left open is whether the site should have
one answer for both media boxes instead of two, which is a question about who else draws
`blockFrame` rather than about the clip alone.
