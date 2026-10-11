---
name: Publishing Is a Mise Task
title: Publishing is a mise task and cannot become a CMS button
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `spec/architecture/local.md` draws the line plainly: a view that has found
outstanding work shows the command that closes it, and that command becomes a button only once
the operation has moved below both shells and the task substrate can report its progress and
refuse a second copy. `publish` is `mise run publish web` and lives below neither shell, so the
CMS may name it and may not run it.

It is exactly the class that rule was written for. It runs for as long as the corpus takes to
compile, it writes into `data/` and then across the network, and two copies racing over one tree
is the failure the substrate exists to refuse. It differs from the paid sweeps in only one way:
it spends time rather than money, which makes an accidental second run cheaper, not safe.

**What deciding it would cost.** Moving it means an in-process application operation in Rust with
a TypeScript compile behind it, which is the subprocess boundary
web's `spec/i18n/segments.md` already accepts in the other direction and cms.md has not
ruled on in this one. Where the compiler lives is the same question arriving from the other side
-- and answering either one first mostly decides the other. Neither is worth
taking while the publish path is still new enough that its shape may move.
