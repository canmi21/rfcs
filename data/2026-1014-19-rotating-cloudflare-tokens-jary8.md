---
name: 'Rotating Cloudflare Tokens'
title: "The platform's three Cloudflare tokens are made, rotated and handed out by hand, and whether the platform mints and rotates them itself is open"
status: draft
supersedes: []
blocked-by: [231yq]
commits: []
references: []
---

## Question

Three Cloudflare tokens do the platform's work at the edge, each made in Cloudflare's dashboard and
kept in the platform's sops file:

| Token                      | Who uses it                                        | Where it is put                          |
| -------------------------- | -------------------------------------------------- | ---------------------------------------- |
| `CLOUDFLARE_WORKERS_TOKEN` | the deployer, deploying every Worker               | the deployer's `secret.env`, by hand     |
| `CLOUDFLARE_NODES_TOKEN`   | `mise run tunnel`, a node's tunnel and VPC service | the laptop, decrypted when the task runs |
| `CLOUDFLARE_ZONES_TOKEN`   | `mise run rules sync`, every zone's firewall       | the laptop, decrypted when the task runs |

Rotating one is the dashboard, `mise run secret`, and the deployer's `secret.env` written by hand --
row S6 of platform's `spec/architecture/operations.md`, with no RFC. It is rare, which is the risk:
nothing says when a token expires, and the steps are remembered rather than run.

The Workers token cannot be the account's. It is a token of the author's user because binding a VPC
service is checked against the user behind the token, and an account-owned token has none: the
console's first deploy, on 2026-10-07, was refused with code 10196 until it was replaced --
platform's `spec/architecture/deployer.md`, "Credentials". Each token is cut to what its user needs,
and "the account's owner login is never used by a script" -- `firewall.md`.

RFC 231yq takes the values out of git as tier 1, and once host reads `secret.env` from the secret
service a rotation is one write there. That leaves making the token, and knowing it must be made.

The ways in sight, and what each costs:

- **Rotated by hand, through the secret service**: the dashboard makes it, the console writes it,
  every reader follows. The expiry is recorded with it and the console says when one nears. The
  least to build. Against it: the dashboard stays, which RFC bz8nz counts as hand.
- **The platform mints its own**, with a token of the author's user allowed to create tokens: it
  makes each working token with an expiry, writes it to the secret service, and revokes the one
  before once nothing uses it. Against it: a token that makes tokens can make any token the user
  can, so it is the one credential worth stealing, and it must be held where tier 0 is or above.
- **Short-lived tokens made per use**, a task's token living minutes. Against it: the deployer and the
  rules sync both run unattended, so the minting token is online all the time.
- **Fewer tokens**: the zones and the nodes tokens folded into the deployer's, if the edge's resources
  become the deployer's to apply, RFC 325qu. Against it: one token that can do all three.

What would decide between them: whether a token able to create tokens may ever be online; what
expiry Cloudflare's tokens are given, which sets how often this happens; and how RFC 325qu places
the edge, which decides whether there are three users of a token or one.
