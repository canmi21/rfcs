---
name: Bare Artifact Names Retire
title: 'app_of stops reading a bare deploy-<app> as arm64 once no run naming its artifacts so is fresh enough to deploy'
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

An artifact named `deploy-<app>`, without its architecture, is read as arm64 -- infra's
`spec/architecture/host.md`, "The machine pulls; nothing pushes into it". How long is that kept?

## Decision

- **`app_of` stops reading a bare `deploy-<app>` as arm64** once no run that still names its
  artifacts that way is fresh enough to deploy.
