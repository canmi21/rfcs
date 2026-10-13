---
name: 'Images in Object Storage'
title: "A run's images are kept in an S3-compatible bucket outside the platform, two versions of each, and GitHub keeps only the run"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Every image CI builds is uploaded as an artifact of its run, and host downloads it from GitHub --
infra's `spec/architecture/host.md`, "The machine pulls; nothing pushes into it". On 2026-10-13
every upload began to fail, `Artifact storage quota has been hit`: the organization is on GitHub's
free plan, whose private repositories include 0.5 GB of Actions storage counted in GB-hours across
the month, and the month had used 6569 GB-hours. Live artifacts were 35 GB, almost all the
platform's, `shot` alone 335 MB per architecture; one build of every image is 2.7 GB for 56
images. A busy day uploads 8.6 GB, and the first full build 21.8 GB. Deleting artifacts gives back
no hours already counted, so nothing deploys through CI until the month ends or storage is paid
for.

The author asked whether a self-hosted git and CI would do, and named the trap: a platform that
builds and stores its own images cannot deploy the fix for itself when it is down.

## Decision

Decided with the author on 2026-10-13.

- **Images go to an S3-compatible bucket, not to GitHub and not to the platform.** CI still runs on
  GitHub's runners and GitHub still says which run succeeded; only the bytes move. A bucket held by
  a provider outside the platform keeps the way to deploy a fix open while the platform is down.
  Which provider is the author's, chosen apart; nothing is written against one, and the bucket is
  reached by the S3 API alone, its endpoint, region and bucket configured.
- **Two versions of each image are kept: the newest and the one before.** After a run uploads an
  image, the versions of that repository's app and architecture older than the newest two are
  deleted. A day's volume then cannot outgrow the bucket: two of every image is at most 5.4 GB
  today. Older versions are not needed to go back, since host keeps the previous image on the node.
  A time limit was rejected: one busy day outgrows any short one.
- **The key is the run's**: `<owner>/<repo>/<run>/<artifact>`, the artifact named as today,
  `deploy-<app>-<arch>`, beside the run's `declarations` and a `manifest.json` listing each object's
  SHA-256.
- **Two credentials, never one.** CI holds a key that writes, lists and deletes, as a repository
  secret; each node's host holds one that reads, in its `.env`, written by `mise run node`. Neither
  is printed or committed in the clear.
- **host checks the run with GitHub and the bytes with the manifest.** It still asks GitHub whether
  the run is its source's `deploy.yml` on `main`, finished and successful, and runs nothing
  otherwise; it then reads the run's manifest and declarations from the bucket and holds every
  image to the manifest's digest. Whoever holds the write key can replace an image, as whoever holds
  CI can today, so that key is kept as CI's own.
- **A run's images gone from the bucket are a run whose artifacts expired**: catch-up and the
  newest-image-within-the-window rules read the bucket instead of GitHub's artifact list, and an
  image deleted as a third version is passed over, as an expired one is.
- **GitHub uploads no artifact**, so the quota and its month no longer stand between a push and a
  deploy, and host no longer spends the shared API limit downloading.
- **Later, not now:** storing images by layer, so a base shared by many images is kept once.
