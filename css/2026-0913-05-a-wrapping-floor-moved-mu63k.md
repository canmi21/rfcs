---
name: A Wrapping Floor Moved
title: A wrapping floor moved and the language override on top of it could not
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `libs/prose/src/toc.svelte` writes its entry labels with
`overflow-wrap: anywhere` and then, for Chinese and Korean, `word-break: keep-all` on top of it.
The comment beside the second calls the first its floor, and it is: a Han run with no space in it
still breaks wherever it must.

Both decide how the text looks and neither moves an element, so under
web's `spec/architecture/css/layers.md` both are the visual layer's. The floor moved. The
override did not, because it is reached through `:lang(zh)` and `:lang(ko)`, and nothing on this
site has established whether StyleX takes a functional pseudo-class as a condition key. Reading
does not settle it -- the plugin either emits the rule or drops it silently -- and the migration's
gate does not either, because no entry in the corpus wraps in Korean and the Chinese one that does
would have to be driven at a width that makes it wrap.

The rendering is unchanged: the two are different properties, the scoped rule is unlayered, and the
interaction between them is the layout algorithm's rather than the cascade's. What is left is one
mechanism written in two places, and the set rule in web's `spec/architecture/css/migration.md`
does not cover it -- that rule keeps a group together only where something else already writes the
group as a unit, and nothing writes `overflow-wrap` beside `word-break`.

Deciding it costs a build and a look at the emitted sheet. It is the same question the first entry
in this file is holding one level up: `utilities.css` writes `line-break` and `word-break` against
`:lang(ja)` and `:lang(ko)` for the article prose, so whether the vocabulary can become typed
depends on the same answer.
