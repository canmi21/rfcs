---
name: Apps Apart From Infrastructure
title: An application has no way to stand apart from the infrastructure it is built on
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The gateway gives the service layer one place for its hosts, its files and its rules. The
application layer -- the site, the status page -- has none: each answers its own robots, its own
security.txt, its own canonical and its own mirrors by calling the libraries, and nothing says
which of its concerns are its own and which it only borrows from the platform.

**Undecided: what separates an application from the infrastructure**, so that the questions only
an application asks -- which of two hosts a page is indexed under, what a page's title is -- stay
with it, and none of the platform's leaks into it.
