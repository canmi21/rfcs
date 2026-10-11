---
name: Two Filename Conventions
title: "The visual layer has two filename conventions and only one of them is the compiler's"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

vocabulary.stylex.ts in `@canmi/kit` carries a vendor's name in its
filename because `defineConsts` refuses to hash a module spelled any other way, and its own doc
comment says exactly that: the filename is the compiler's, not this repository's. The module
holding composed styles has no such requirement -- `stylex.create` hashes from the declaration and
never reads the filename, which is why a create block declared in one module and imported into a
component works at all -- so it is `surfaces.ts`, and the visual layer now has two filename
conventions with nothing saying which applies to the next file.

The rule that settles it is already written and is not about StyleX. The workspace's `naming.md`
lets a framework own a filename where a renamed file simply stops working, which is exactly
`vocabulary.stylex.ts` and exactly not `surfaces.ts`; where that exception does not apply, the
vendor rule does, and a vendor name belongs only in the thin layer that binds to the vendor. Its
test is the reusable part: **if the vendor were replaced tomorrow, how many names would have to
change? One. If the answer is more, the vendor has leaked past the boundary.** Applied here the
answer is zero names for a suffix-free module and one for a suffixed one, which is why the suffix
was refused.

What is unrecorded is the rule rather than the outcome. Nothing in `spec/` says that a `.stylex`
filename is a compiler requirement being obeyed rather than a house convention being followed, so
the next module in this layer is a coin toss -- and the next person to meet the question will be
meeting it about some other vendor, where the same test gives the answer and no file points them at
it.
