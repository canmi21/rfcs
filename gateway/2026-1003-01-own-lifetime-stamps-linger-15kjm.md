---
name: Own Lifetime Stamps Linger
title: The CDN and the alias layer still stamp their own lifetimes behind the gateway that stamps them
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Both stand behind the gateway now, which answers their hosts -- `cdn.monoflake.com` and `ill.li` --
their CORS, their host files and the path rule, and stamps what leaves them from their declarations.
Each still stamps its answers itself as well, in its `cache.ts`: the gateway writes over it, but the
CDN keeps derived pictures in its own cache by that stamp, so it is not idle there. Whether the CDN's
own cache reads its lifetime from the declaration instead, and the alias layer's stamps go, is the
gateway's cleanup pass's to settle.
