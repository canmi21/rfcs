---
name: The Label Bot
title: "A bot turns a console change to a label into a pull request on the app's repository"
status: declined
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

A label is declared in the app's repository, and the console is to write it there -- infra's
`spec/architecture/host.md`, "A label is declared in the repository, and the console will write it
there". How does a change made in the console reach the repository?

## Decision

- **A bot turns the console's change into a pull request**, so git stays the record of it.
- **Until the bot exists a label is changed in `service.toml` by hand.**
