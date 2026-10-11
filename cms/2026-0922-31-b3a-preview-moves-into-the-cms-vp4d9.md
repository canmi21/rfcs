---
name: 'B3a: Preview Moves Into the CMS'
title: 'A CMS route reads a draft the way the site would, and the draft root retires with it'
status: landed
supersedes: []
blocked-by: [p5tma]
commits: [web@5f30b335, web@f1aaf686, web@2c993292, web@fe85841c]
references: []
---

## Question

A draft was previewed by publishing it to a draft root the site read. B3a of web's milestones.

## Evidence

B3a was marked done once while half taken -- web's `spec/architecture/collection.md`.

## Decision

- **A route in the CMS previews a draft the way the site renders a published article**, on the page
  an article is read on rather than an approximation of it, at `/preview/[rid]`.
- **The draft root retires**, which the preview replaced.
