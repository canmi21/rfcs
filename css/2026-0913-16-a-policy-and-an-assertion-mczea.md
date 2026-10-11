---
name: A Policy and an Assertion
title: One property has a measured policy in one layer and a flat assertion in the other
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `apps/site/src/styles/utilities.css` answers `text-wrap` for article prose with a
rule conditioned on language and on width. `.article-content` sets `wrap` at rest and takes
`pretty` only for English, German, Spanish and French, and only above 45rem. Both exclusions are
measured rather than omitted: in WebKit at 354px `pretty` adds lines in every language, eleven in
German and forty-two in Japanese, and roughly doubles the mean right-hand gap everywhere; on
Japanese it takes that gap from 1.6% to 7.9% and the loose lines from 1 to 58.
web's `spec/styling/prose.md` argues each of those numbers, and the comment beside the rule
calls the narrow column the correction a phone forced.

Four license pages answer the same property with no condition at all. `summary` in
`routes/licenses/[license]`, `routes/licenses/pkgs`, `routes/licenses/pkgs/[registry]` and
`routes/licenses/pkgs/[registry]/[...package]` is the same three declarations in the same order --
`lineHeight: line.relaxed`, `textWrap: 'pretty'` and `color: var(--color-text-soft)` -- and the
middle one is what the rule above spends a paragraph deciding not to give to Japanese or to a
narrow column.

Nothing overrides anything today, because the two reach different elements: one is article prose
and the other is a license page's summary line. What is unresolved is that one property now has a
policy in one layer and a flat assertion in the other, and the flat one is what a reader gets on a
phone.

**The reach is larger than those four and has not been counted.** `color: var(--color-text-soft)`
beside `textWrap: 'pretty'` recurs on six components across eight style keys; the members of that
group were not enumerated when it was found, so six is a floor rather than the number. Anyone
deciding this counts them first.
