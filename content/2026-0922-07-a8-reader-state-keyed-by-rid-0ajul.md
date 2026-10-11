---
name: 'A8: Reader State Keyed by Rid'
title: 'Counters, subscriptions and later comments are re-keyed by rid once, by the fold rule'
status: accepted
supersedes: []
blocked-by: [9adnf]
commits: []
references: []
---

## Question

The plan's milestones are agreed in order, each saying what it changes and what it cannot start before -- web's `spec/architecture/collection.md` holds the model they assume.

## Evidence

A8 is deliberately not near-term. web's `spec/engagement.md` records what a re-key already
cost once -- six stranded rows in the fifty-four minutes between a migration and the code that
wanted it -- and the rule it produced is that such a migration folds rather than skips and runs
twice. That is a piece of work with its own rehearsal, and it does not belong inside a week that is
also moving the corpus.

## Decision

- **Counters, subscriptions and later comments are re-keyed once**, by the fold rule in web's
  `spec/engagement.md`.
