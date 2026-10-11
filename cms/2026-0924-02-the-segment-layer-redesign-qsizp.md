---
name: The Segment Layer Redesign
title: The segment layer waits for its redesign
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `spec/i18n/segments.md` keyed a translation to the hash of a block's canonical text,
and stored every article canonical so the hash would hold. Both are retired: an identity taken from
a hash of text moves whenever the text is respelled, and the editor now saves what the author wrote.
Nothing replaces them yet, so no new article is translated and no existing one gains a locale. It
does not block the editor or writing in the source language.

**What is known of the replacement**, from the user and not yet designed: segments are defined by
structural equivalence -- what a block is, how many lines it exports and what its text is -- rather
than by bytes. Before anything moves, three things are exported from the current sidecars: every
translated text on its own, the same text joined back into articles, and the metadata as JSON, so
the redesign starts from a copy it cannot damage. The i18n structure itself may change with it.

**What deciding it would cost.** The export is mechanical. The redesign is not: every sidecar was
written against canonical hashes, so moving them is a migration, and `validate.rs` and the runners
read the old shape.
