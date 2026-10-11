---
name: "One Layer for Every Host's Files"
title: 'robots.txt and security.txt are owed by every page, and whether one layer answers them for every host is open'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Every independent page a project deploys answers both files -- the workspace's `spec/robots.md` --
and today each project declares its own hosts: web its page hosts in `libs/robots`, the platform its
service hosts through the gateway. The API side already has one layer in front of every service that
answers these files once, the gateway. Whether every deployed page gets a layer in front of it the
same way, so the files are declared once rather than per project and per host, is not decided, and
neither is what that layer would be.

## Evidence

web's service domains' pages, `landing`, answer neither file yet, and are waiting on this.
