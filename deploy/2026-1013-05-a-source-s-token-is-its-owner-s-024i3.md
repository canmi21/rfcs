---
name: "A Source's Token Is Its Owner's"
title: "The platform reads its own repositories with a machine user's token, and an organization's repositories with that organization's token, held by the platform"
status: accepted
supersedes: []
blocked-by: [231yq, nzscl]
commits: []
references: []
---

## Question

Every host and the deployer read GitHub's Actions with the author's own token. On 2026-10-13 the
author's agents spent its rate limit twice in an hour, and every node's deploys stopped with it. The
author made a machine user, `canmiwu`, that day, and asked whose token reads which repository.

## Decision

Decided with the author on 2026-10-13.

- **The platform's own repositories, `monoflake/*`, are read with the machine user's token**,
  `GITHUB_ACTIONS_TOKEN`, so nothing a person or an agent does spends what the platform's deploys
  need.
- **An organization's repositories are read with that organization's own token.** The author's
  `canmi21/*` keep the author's `GITHUB_ACTIONS_TOKEN_CANMI21`: they change seldom, and are the
  author's, not the platform's.
- **That token is to be held by the platform, not by its repositories.** Once accounts exist, the
  author is a user who holds the organization `canmi`. Its sources and the token that reads them are
  that organization's: a tier 1 secret of its scope in the secret service (RFC 231yq), never in
  `secrets.json`. The platform's own token is then the only one its repositories keep.
