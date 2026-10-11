---
name: Deploy Sources in Code
title: 'Which repositories deploy, and with whose token, is written into code, and how it moves into the cluster is open'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

A repository is a source of deploys in three places, each fixed until something is redeployed: the
`hook` forwards a run only from `DEPLOY_SOURCES` in `@monoflake/sdk`; infra's `mise run node` writes
the same list into every host's `.env`; and host fetches every run's artifacts with one
`GITHUB_ACTIONS_TOKEN`, monoflake's, so a repository another owner holds cannot be read at all. Found
on 2026-10-08, when the author asked to deploy `qq` from a private repository of their own account to
`sha`: each new source is an edit to code, a redeploy of the hook, and a new `.env` on every node.

The author's direction: a source and the token that reads it are set while everything runs, as the
platform's configuration in the cluster, and only what is fixed by nature -- an image's architecture,
a role a node grants -- stays in code.

## Options

What deciding it involves:

- **The record**: each source as `owner/repo`, the owner's token, and which nodes take it, in the
  cluster -- platform's `spec/architecture/databases.md` -- written by the console or a task, never by
  a commit.
- **The tokens are secrets at rest**: encrypted in the cluster, or kept apart from it, and whether a
  node holds only the tokens of the sources it takes.
- **host cannot need the cluster to deploy**: the database is itself deployed by host, so a node keeps
  the last sources and tokens it was given, and deploys from them while the cluster is down.
- **The hook needs no list**: a run signed with the webhook's secret could be forwarded whatever its
  repository, every node deciding by its own sources, as host already does.
- **An app belongs to the scope its source carries**, decided on 2026-10-08: the scope of each source
  is part of the same record.
- **The same layer holds the node list**, as the question of a node added in seven places asks.
