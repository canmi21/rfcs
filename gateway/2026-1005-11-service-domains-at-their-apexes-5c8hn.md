---
name: Service Domains at Their Apexes
title: "The service domains answer at their apexes, the hosts' half of what web's landing pages decide"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `spec/architecture/landing.md` decides that each service domain answers a page of its own at
its apex. What do the hosts need for it?

## Decision

- **`symlink.si` gives its host to a static site on the CDN and keeps the names on a route.** The apex
  gets a record proxied by Cloudflare, `/` and everything that is not a name reach the site, and the
  names the alias layer answers stay on the gateway's Worker as a route laid over the host -- in place
  of the custom domain that takes the whole host today, platform's `spec/architecture/gateway.md`,
  "The gateway declares its hosts in its `wrangler.jsonc`".
- **`monoflake.net` sends to `monoflake.com`**, not to the site: its redirect in
  `rules/monoflake.net` changes its target.
- **`monoflake.com` and `ixc.one` get their apex records**, so the page each is to answer can be
  reached; `www.ixc.one`'s redirect to its apex leads nowhere until then.
