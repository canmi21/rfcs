---
name: cdn.ffoni.com Bound Back
title: 'cdn.ffoni.com is bound back until the corpus is republished without it, then released again'
status: accepted
supersedes: []
blocked-by: [7qrfw]
commits: []
references: []
---

## Question

`ffoni.com` was released on 2026-10-04 once rdm had moved, and the gateway stopped answering
`cdn.ffoni.com`; its proxied record stayed, so the host answered `522`. The site's live root,
published 2026-10-01, still names it: 30 of the objects its articles point at carry `cdn.ffoni.com`
addresses, and the same articles compiled today carry none. Nothing republishes the root until the
CMS's migration closes the gap, and that migration waits on the platform.

## Decision

- **The host is bound back as it was before**, done on 2026-10-08: a profile read as
  `cdn.monoflake.com` at `v3`, by one route in the zone. `api.ffoni.com` and `rules/ffoni.com` stay
  gone; whatever the zone still carries on Cloudflare is as the release left it.
- **It is released again once the corpus is republished without the domain**: the profile, the
  route, `GATEWAY.retired`, and `ffoni.com` among the platform's zones in the deployer's admission
  test are deleted, as on 2026-10-04 -- after a count of what still asks the host, since an article
  somebody saved is a caller nobody here can move.
