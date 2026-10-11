---
name: An Old Run Can Roll Back
title: "An old run's notice can roll an app back, and where host keeps the newest run it deployed is open"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

host takes each `(repository, run)` once per process, and again if taking it failed, but never asks
whether a run is newer than the one that deployed the app. A notice naming an old successful run --
after host restarts, or for a run it never took -- deploys that run's image over a newer one.

The platform's deployer refuses a run that is not newer than the last that deployed its Worker
(platform's `spec/architecture/deployer.md`); host keeping the same mark per app, and refusing a run
not newer than it, is the fix in direction. Where the mark is kept, and how an operator's explicit
rollback stays allowed past it, are undecided.
