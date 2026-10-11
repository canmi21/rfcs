---
name: Cloudflare Is the Entrance
title: "A caller anywhere meets one gateway through Cloudflare, and the house's gateway retires"
status: landed
supersedes: [6fiei, 8ndqx]
blocked-by: []
commits: [platform@9baec95e]
references: []
---

## Question

A `shot` task taken through Cloudflare on `tyo` was not found from the LAN: the house answered the
gateway's names with its own gateway, which saw another order of placements. The node at home may
go offline, and the control plane is on `tyo`. Is there to be one gateway or two?

## Options

| Gateways                           | Against it                                                           |
| ---------------------------------- | -------------------------------------------------------------------- |
| One at the edge and one at home    | Two orders of placements, and the LAN loses the API when home is off |
| One, at the edge, for every caller | A device at home pays the round trip through Cloudflare              |

## Evidence

platform's `spec/architecture/gateway.md`, "Inside a node, its own services answer locally".

## Decision

One, at the edge, for every caller.

- **A device at home asks the public names through Cloudflare** as every other caller does; the
  LAN's resolver no longer answers `api.monoflake.com` or the service domains with the node.
- **The gateway and `quota`'s node copy retire**, E11 with them; the resolver keeps the private
  suffix of E12 and nothing of the gateway's names.
- **A container asks the private side, its own node's Caddy**, and Caddy hands any scope not on that
  node to the public gateway with `INTERNAL_TOKEN`.
