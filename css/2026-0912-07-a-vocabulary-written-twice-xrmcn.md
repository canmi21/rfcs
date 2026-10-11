---
name: A Vocabulary Written Twice
title: 'A shared visual vocabulary, arrived at by two people writing it separately'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The two directory pages were migrated an hour apart by different hands, and seven of the nine
style objects on the second are the first's character for character: the page ground, the trail
link, the heading, the section heading, the row with its radius and hover fill, the dashed leader
and the tabular count. Fifty-seven of eighty lines in one module block are shared verbatim with
another file. Nobody copied deliberately; both were translating the same markup under the same
rules and arrived at the same place.

That is the extraction threshold reached from an unusual direction. The workspace's rule says a
thing is extracted when it acquires a second consumer, and normally the second consumer is a
decision somebody makes. Here it is a measurement: the duplication already exists, in two files
that will drift the first time one of them is edited alone.

Tailwind's `transition-colors` list is the same problem at its smallest. It is now written out
five times -- the two directory pages, the package page, the article section and the newsletter --
as ten properties including three `--tw-gradient-*` custom properties that are another framework's
private variables and that this site never sets. Whether they belong in our source at all is a
second question, and it has to be answered wherever the string finally lives or the two answers
will disagree.

So there are two things to settle and one place to settle them. **Where a shared visual constant
lives**: a `.stylex.ts` module components import, which is the same boundary question as
`@canmi/ui/primitives` above and should be answered with it rather
than beside it. And **what the shared surfaces are called**, which is the part that cannot be
mechanical, because a name that describes the markup it came from stops being true the third time
it is used.

The timing function and the duration are literals in every migrated file today and follow the
same string wherever it goes.
