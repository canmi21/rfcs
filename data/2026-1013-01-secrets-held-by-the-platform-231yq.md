---
name: "Secrets Held by the Platform"
title: "The platform holds every secret but the few that start it, sealed by a key two of the three cores rebuild and one offline key opens alone"
status: accepted
supersedes: []
blocked-by: [ja5hm]
commits: []
references:
  - url: https://developer.hashicorp.com/vault/docs/concepts/seal
    took: 'a root key split by Shamir''s scheme and held only in memory once rebuilt, every secret encrypted beneath it'
  - url: https://getsops.io
    took: 'the secrets that start the system, committed as ciphertext to an age key, which is how they are kept today'
---

## Question

Every secret is in some repository's `secrets.json`, encrypted with sops to the author's age key.
What the code needs and where it runs are leaving git for the platform -- RFC ja5hm -- but a secret
cannot be written from the console, and every change to one is a commit and a redeploy. The author
asked on 2026-10-12 for the platform to hold its secrets. A store of secrets seems to need storage of
its own, and so to wait on the bucket, RFC vh1bl.

## Decision

Decided with the author on 2026-10-13, as the most pressing thing missing.

- **Secrets are of two tiers.**
  - **Tier 0** is the few that start what holds the rest: the database's and the quorum's
    credentials, the store credentials its backups are written with, the tunnel's token, and each
    core's share below. It stays in `secrets.json`, sops over age, committed to GitHub. GitHub is
    trusted until the platform is stable enough to hold itself, a bet that it fails less often than
    the platform does.
  - **Tier 1** is every other secret, held by the platform and written from the console. It never
    enters git.
- **The store of secrets needs no bucket.** Secrets are a few kilobytes each. Their ciphertext is
  rows in the platform's Postgres, written as what must not be disputed, confirmed by two cores --
  RFC bhybp. The chain runs one way: tier 0, then the platform's Postgres, then the secret service,
  then tier 1, then buckets and apps. A bucket's store credentials are tier 1, so a bucket depends on
  secrets and never the reverse.
- **Every secret is encrypted under a key of its own, and that key under the root key**, which
  lives only in the memory of the service that rebuilt it.
- **The root key is split two of three among `tyo`, `sin` and `rdu`** by Shamir's scheme. The node
  task writes each core's share to its disk, outside every container. Any two shares rebuild the
  root key, and from it any share lost, as a majority decides.
  - Normally all three cores answer.
  - With one core down, the two left unseal.
  - A lone core cannot unseal by itself.
- **One recovery key opens the whole lock**: an age key apart from the one sops uses day to day. It
  is kept on the author's machine, never online, never sent anywhere, and used for nothing else.
  The root key, encrypted to it, is committed as ciphertext. When fewer than two cores are left, the
  author unseals the lone one by hand with it. Every other unseal needs no person.
- **A node is given only the secrets of the apps placed on it**, read with its own node token, as
  RFC 8vy7x gives it only its own wanted state. host writes them into each app's `secret.env` as it
  does now, and keeps what it last read. A secret service down stops a change to a secret, never a
  node.
- **Tier 1 moves out of git as placements did**: each secret marked tier 0 or 1, tier 1 imported
  into the service, then deleted from `secrets.json` once the service answers it.
- **This comes before RFC 8vy7x.** Signing protects what a node is told to run. A secret's
  confidentiality comes from each node reading with its own token, and the author's own nodes are
  the only ones there are.
