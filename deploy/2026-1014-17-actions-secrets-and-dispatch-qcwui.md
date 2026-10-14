---
name: 'Actions Secrets and Dispatch'
title: "CI's secrets and variables are set, and a run is dispatched or re-run, from GitHub's UI or gh on the laptop, and whether the platform does both through GitHub's API is open"
status: draft
supersedes: []
blocked-by: [024i3]
commits: []
references: []
---

## Question

CI stays outside the platform by choice: GitHub Actions builds each image and Worker and uploads it
to the artifact bucket, RFC q28g5. What CI is given, and when it runs out of turn, are still set by
hand, two rows of platform's `spec/architecture/operations.md` with no RFC:

- **CI2, its secrets and variables.** `ARTIFACTS_WRITE_KEY_ID` and `ARTIFACTS_WRITE_SECRET` as
  secrets, `ARTIFACTS_ENDPOINT`, `_REGION`, `_BUCKET` and `_VIRTUAL_HOST` as variables, read by
  every repository's `deploy.yml` -- infra's at lines 117 to 122, the platform's at four steps from
  line 132 -- and set by `gh secret set` and `gh variable set` per repository. The bucket moved to R2
  on 2026-10-14, RFC 7cwmp, and the store keys were rolled on 2026-10-13 and again on 2026-10-14,
  CI's write key set again in each repository by hand.
- **D9, a run dispatched or re-run.** A run lost or cancelled is re-run from GitHub's UI; one Worker's
  run was dispatched by hand when `sin` was brought up on 2026-10-12, RFC dk6wz's evidence; a release
  of `@monoflake/sdk` held back by a failed test was run again by hand on 2026-10-08 -- platform's
  `spec/repository.md`, "Publishing". The catch-up of every 15 minutes, RFC tkuxb, covers a lost
  notice, not a run never made.

RFC bz8nz forbids gh from a laptop and a GitHub secret set by hand. RFC 024i3 has the platform read
its own repositories with a machine user's token, held by the secret service, and RFC rpkku asks
what a GitHub App the platform runs would do first; neither sets CI's secrets or starts a run.

The ways in sight, and what each costs:

- **The platform writes CI's secrets and variables through GitHub's API**, from a store's credentials
  held in the registry, RFC vh1bl: rolling the bucket's write key writes it to every repository that
  builds, as host writes an app's `secret.env`. Against it: the token doing it needs write access
  to Actions secrets, more than 024i3's read; GitHub keeps a copy of a key whose home is the secret
  service.
- **CI takes nothing long-lived**: each run trades GitHub's OIDC token for a short-lived write key to
  the bucket, minted by the platform or by the store's own federation where it has one. CI2 leaves
  the count instead of being automated. Against it: an exchange the platform must run and keep up,
  on the path of every build; whether R2 can take such an exchange is unread.
- **Dispatch and re-run as console actions**, through GitHub's API with the machine user's token or
  the App's, recorded as a task like any action, RFC cpc2b. Against it: a token that may start
  workflows, a write on every repository it reaches.
- **A GitHub App for both**, folding this into RFC rpkku: one installation per organization, its
  token minted per call, scoped to what it is installed on. Against it: an App to register and
  host, and a webhook to receive beside the hook's.

What would decide between them: whether the platform may hold a token that writes to GitHub, or only
ones that read; whether R2 or a store the platform runs can mint a per-run key from GitHub's OIDC
token; and whether RFC rpkku's App is built for other reasons, which would make it the carrier for
both.
