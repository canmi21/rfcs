---
name: 'A2: The Records Are Imported'
title: 'What the YAML and JSON records hold moves into the authored database, and the files retire'
status: accepted
supersedes: []
blocked-by: [67ity, 5vf5e, rjw0s]
commits: []
references: []
---

## Question

The plan's milestones are agreed in order, each saying what it changes and what it cannot start before -- web's `spec/architecture/collection.md` holds the model they assume.

## Evidence

**A2 is half taken, and this row was marked `done` before that was checked.** What the records
hold did move in -- `import.ts` writes every resource, content, tag and paid description into the
collection, and the schema tests hold it there. The files did not retire: `media.yaml`,
`tags.yaml` and `metadata.json` are still on disk and still read, by around twenty modules
including the publish pass, `local`'s own commands and the preview's compile. They are the source
of truth and the collection is the copy, which is the wrong way round for a row that says they
retire.

Retiring them is a reader migration, not a deletion, and it does not wait on A5: those are the
article's fields and these are the media and tag records, two sets of data with two sets of
readers. What it waits on is stated below, and it is a decision rather than an amount of work.
These three files are also C5's first row, so that milestone inherits whatever this one settles.

## Decision

- **What the YAML and JSON records hold moves in, and the files retire.** Retiring them is a reader
  migration: the around twenty modules that read `media.yaml`, `tags.yaml` and `metadata.json` read
  the collection instead.
