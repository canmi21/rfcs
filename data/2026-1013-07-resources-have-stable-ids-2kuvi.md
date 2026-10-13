---
name: "Resources Have Stable Ids"
title: "Every resource the platform holds has an id that never changes, its name a label that may, and everything that refers to it holds the id"
status: accepted
supersedes: []
blocked-by: [cpc2b]
commits: []
references:
  - url: https://developers.cloudflare.com/kv/concepts/kv-namespaces/
    took: 'a namespace bound by a 32-character hexadecimal id, its name free to change'
---

## Question

A resource in the registry is known by its name -- RFC cpc2b -- so renaming one changes every binding,
release and history row that names it. The author asked on 2026-10-13 whether buckets, workers and
everything the platform holds should be bound by an id instead, as Cloudflare binds them.

## Decision

Decided with the author on 2026-10-13.

- **Every resource has an `id`**: 128 random bits as 32 lowercase hexadecimal digits, a version 4
  UUID without its hyphens. It is made where the resource is made, needs no register to be unique,
  and never changes. Version 7 is not used: it orders ids by time, which this size does not need,
  and tells when each was made.
- **Its `name` is a label**, unique within its scope and kind, and free to change.
- **Everything that refers to a resource holds its id**: a binding, a release, a layout's store, a
  ledger task, a history row. A rename touches nothing that refers to it.
- **A resource leaves by being marked `retired`, and its id is never given again.**
- **A node keeps its name where others hold it**: DNS, the tailnet and the edge's bindings name
  `tyo`, so renaming a node is an act of its own that changes them, not a label edited.
- **The API answers `/v1/<kind>/<id>`, and finds by name with `?name=`.** The registry's name keys
  move to ids by expanding first: an `id` added and filled in, both ways answered, then the names
  contracted once nothing reads by them.
- **The content `rid` stays what it is**, five base36 characters allocated against its register,
  since a person reads it in an article's source; a resource id is read by machines.
