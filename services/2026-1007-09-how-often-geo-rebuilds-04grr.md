---
name: How Often Geo Rebuilds
title: "geo's data is as new as its last build, a month at worst, and how often it is rebuilt and by whom is open"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

geo's GeoNames and GeoLite2 are fetched as its image is built -- platform's
`spec/architecture/geo.md`, "GeoLite2 is fetched as the image is built" -- so they are as new as the
last build: a push touching geo, or `deploy.yml`'s run on the first of each month. MaxMind publishes
GeoLite2 twice a week, so an answer can be a month behind. Rebuilding more often redeploys the whole
image to every node each time, whether or not the data changed.

Who triggers it is open too: GitHub's own schedule, as now, or the platform's `cron` asking GitHub
for a run, which needs a token in `cron` and, since `cron` runs on every node, the platform's
scheduler to ask once rather than seven times.
