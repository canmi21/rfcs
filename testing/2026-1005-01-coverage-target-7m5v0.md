---
name: Coverage Target
title: 'Every project is to reach 95% line coverage, and how each is measured and held to it is open'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The target is the whole workspace's, every repository below it included: 95% of lines covered by a
test that would fail if they broke. It is where lib's `axum-governor` already holds itself, in its
`spec/axum-governor/testing.md`, and nowhere else. What is open is the way there: how coverage is
measured in each language here (Rust, TypeScript, Svelte), whether a gate holds a repository to it
once it gets there or only reports, and the order the repositories are brought up in. There is no
time for it yet, which is why it is a question and not a decision.

## Evidence

How `axum-governor` reads the number is the one reading written down so far: a function that
orchestrates tested functions is covered by testing its orchestration -- call order,
short-circuits, data threading -- not by re-testing every branch of what it calls; and an I/O-heavy
module may fall below where the uncovered branches are error paths with no observable behavior,
saying so in the module.
