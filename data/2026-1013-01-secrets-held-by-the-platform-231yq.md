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
    credentials, the store credentials its backups are written with, and the tunnel's token. It stays in `secrets.json`, sops over age, committed to GitHub. GitHub is
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
- **The root key is split two of three among `tyo`, `sin` and `rdu`** by Shamir's scheme. The shares are
  made on the cores when the lock is first set, each kept on its core's disk outside every container,
  and never in git: the sops key day to day, with GitHub, must not rebuild the root key. Any two shares rebuild the
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
- **The service holds an app's whole environment, each value of one of two classes**, decided with
  the author on 2026-10-13, as a node already splits them into `config.env` and `secret.env`.
  - **`config` is read by anyone who may read the app's values**, at any time, as written.
  - **`secret` is never answered once written**, but to the node it is placed on, which writes it
    into `secret.env`.
  - **A value is `secret` unless its write declares it `config`**, so a token written in haste is
    never exposed by default. A `secret` becomes `config` only by being written again with its value,
    never by a flag turned, so no one shows a secret they could not already have typed.
- **A secret can be compared without being read.** A value offered is checked against the one held,
  in constant time, and the answer is `true` or `false` alone, the value held never replaced. It
  takes the right to write that secret, is limited in how often it may be asked, and each comparison
  is a row of the history -- who, when, which, and the answer -- never the value offered.
- **One API writes values for the console and the command line alike**, RFC cpc2b. A resource's field
  the catalog marks `secret` -- an account's identity, a store's credentials -- is written with its
  value through the registry, which hands the value to this service and keeps only its name, never
  storing, logging or answering the value. The registry writes with a token of its own that reaches
  only the scope it keeps such values in, so the service grants tokens by scope.
- **This comes before RFC 8vy7x.** Signing protects what a node is told to run. A secret's
  confidentiality comes from each node reading with its own token, and the author's own nodes are
  the only ones there are.
