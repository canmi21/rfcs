---
name: Eight License Addresses
title: The license surface is eight addresses and one baked record
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`/licenses`, `/licenses/{spdx}`, `/licenses/pkgs`, `/licenses/pkgs/{registry}`,
`/licenses/pkgs/{registry}/{package}`, `/licenses/{registry}/{name}@{version}.txt`,
`/licenses/full.txt` and `/licenses.txt` are eight public addresses across thirteen route files.
Nothing else this site serves spends that much of its URL space on one subject, and the subject is
a dependency list.

Behind them, the `virtual-licenses` plugin in web's `apps/site/vite.config.ts` bakes
`data/build/licenses.json` into the server bundle. Measured on a production build: 482KB raw and
71.7KB gzipped, the largest chunk the Worker carries after the corpus itself -- ahead of
`index-server.js` at 40.6KB and `surfaces.js` at 36.5KB. Only the metadata travels; the license
texts are already published objects the CDN serves.

What makes it a finding rather than a preference is that the record sits on the wrong side of a
line this repository is in the middle of drawing. It is derived from the lockfile by a pure
function, so nothing is lost by regenerating it, which is the test that sends a generated record
out of git and into R2. It is also the only such record with a consumer at request time. So the
classification reaches it, and the answer it gives -- publish it as an artifact and fetch it like
any other -- is an answer about a surface nobody has decided to keep.

**One piece has already been taken, and it was the expensive one.** The surface had an OpenGraph
card per license, per registry and per package, in each of the nine views: 6804 files and 518 MiB,
87% of everything the bucket held. Those are gone and the routes are not -- see
web's `spec/architecture/media.md`, "The license routes have no card". It narrows
nothing about the addresses; it stops an undecided surface from being the largest thing published.

**The direction is set and waits on the platform.** The surface leaves the site: its routes take
another form, and the page becomes a shared one that `apps/landing` serves, which depends on the
platform's hosts being built first -- see RFC zn0c9 for the layer in front of
every page it may sit behind. What stays open is the shape the addresses take.

**What deciding it would cost.** The intent is two or three addresses rather than eight, and which
ones is open: a single page carrying the directory inline, or a page plus the text endpoints that
exist for machines rather than readers. Whatever survives decides what the record has to be, which
is why it is held out of the move rather than carried through it and rebuilt afterwards. The cost
of waiting is that `data/build/licenses.json` stays in git while every other pure derivation
leaves; the cost of not waiting is migrating a payload onto a surface that is about to lose most
of it.
