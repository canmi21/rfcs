---
name: The New Domains Answer
title: 'monoflake.com, its twin monoflake.net, ixc.one and symlink.si are bound to the gateway'
status: landed
supersedes: []
blocked-by: [xspxi]
commits: [platform@57c1dadb, platform@50f0b731, platform@d67300e7, platform@5e6e53f1]
references: []
---

## Question

The service layer was to answer at its own domains. E6 of the gateway's milestones, the one step
whose order is forced from outside: a zone has to exist before a route or a rule can name it.

## Decision

- **`monoflake.com`, its twin `monoflake.net`, `ixc.one` and `symlink.si` bound to the gateway**:
  DNS, routes and certificates, their zones brought into `rules/`.
- **`monoflake.net` answers as `monoflake.com`'s twin**, a way round a domain that fails.
- **Each zone the gateway owns is routed by one wildcard**, custom domains kept for the apexes.
