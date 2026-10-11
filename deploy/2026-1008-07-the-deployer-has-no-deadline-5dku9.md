---
name: The Deployer Has No Deadline
title: "The deployer's fetches to GitHub carry no timeout, so one hang holds every run queued behind it"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The deployer's fetches to GitHub and to the storage it redirects to carry no timeout, and it deploys
one run at a time, so a GitHub that hangs holds every run queued behind it. Found on 2026-10-09.
Deciding it is a deadline per request, and whether a run that ran out of it is retried or closed as
failed like any other unread run.
