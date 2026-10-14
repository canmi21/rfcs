---
name: 'The Edge as Resources'
title: "DNS records, Access applications, the other Workers' custom domains, the firewall's sync and mail forwarding are set at Cloudflare by hand, and whether they become resources a controller keeps is open"
status: draft
supersedes: []
blocked-by: [cpc2b]
commits: []
references: []
---

## Question

RFC bz8nz counts the platform self-hosted once no act on it needs a provider's dashboard or a
credential on a laptop, and gives an act with no RFC one before it is worked on. Five of the acts
platform's `spec/architecture/operations.md` lists are Cloudflare's edge, and none has one:

| Row | Act                                                       | Today                                                                  |
| --- | --------------------------------------------------------- | ---------------------------------------------------------------------- |
| NW1 | DNS records: apexes, `*` wildcards, a custom domain's own | the dashboard; RFC 5c8hn asks for the apexes' records                  |
| NW2 | Access in front of `*.canmi.app` and the private names    | the Zero Trust dashboard, per new private name                         |
| NW3 | Sending `rules/` to every zone                            | `mise run rules sync` from the laptop, with `CLOUDFLARE_ZONES_TOKEN`   |
| NW6 | Forwarding the security address                           | the Email Routing dashboard                                            |
| W6  | Custom domains of every Worker but the gateway            | the dashboard; platform's `spec/architecture/gateway.md` says it plain |

What the hand costs has been seen once. Attached in the dashboard, `api.ffoni.com` was gone after a
deploy that followed its move from the old API Worker, its DNS record with it, and the first sign
was the host not resolving; since then the gateway declares its custom domains in its
`wrangler.jsonc` and each deploy asserts them -- gateway.md, "The gateway declares its hosts". On
2026-10-04 the zones the gateway owns became one wildcard route over a proxied `*` record, made by
hand, which must exist before the deploy that drops a custom domain on it. The other Workers'
domains are still the dashboard's "and move the same way if one goes".

The firewall is the nearest to done: `rules/` is its one source and the sync is a task, but it "is
run by hand after a change lands" -- platform's `spec/architecture/firewall.md` -- and it is one-way
on purpose, never reading what is live. RFC 8qh17 generates the service zones' whitelists; it does
not say who sends them. Access is the odd one: it stands in front of the console, so the console
writing it could lock its own writer out.

The ways in sight, and what each costs:

- **Declarations in git, applied by CI on push**, as `rules/` is but run by Actions rather than the
  laptop, with DNS records, Access applications and custom domains declared beside the rules. The
  smallest step, and NW3 closes with it. Against it: GitHub holds a token that edits every zone,
  which RFC 231yq's tiers would keep off it; a failure is seen in Actions, not the console; and the
  dashboard can still drift from git unseen.
- **Each a kind in the registry, RFC cpc2b -- a record, an Access application, a Worker's domain, a
  zone's rules, a mail route -- and one edge controller bringing Cloudflare to them**, RFC jln34. The
  console writes them and draws drift, and the token is the secret service's. Against it: a
  controller per Cloudflare API, which must read back as well as write, the firewall's one-way stance
  given up; an import of everything the zones hold now; and a kind per resource.
- **Declarations in git, applied by a platform service rather than CI**: the deployer already holds a
  Cloudflare token and applies what a run's artifact carries, so it could apply a zone's records and
  rules the same way. Against it: the deployer grows past Workers, and its token gains the zone and
  DNS permissions it was kept from -- platform's `spec/architecture/deployer.md`, "Credentials".
- **Some stay by hand, named as bootstrap**: Email Routing changes about never, and Access guards the
  console, so the way to change Access may have to be one the console does not stand behind.

What would decide between them: how often each act happens -- NW1 per domain, NW3 per rule, NW6
almost never; whether the edge's live state should be read back and its drift shown, against the
firewall's choice never to read it; which token may edit the zones and DNS and where it is kept,
RFC jary8 asking how it is rotated; and whether Access can be written from the console without the
console being the only door to it.
