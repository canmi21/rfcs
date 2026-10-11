---
name: Root Blocks in Kit Tokens
title: '`@canmi/kit/tokens` is where a `:root` block goes, and only one kind has been tested there'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The same file sends a `:root` block declaring nothing but custom properties to
@canmi/kit/tokens, "which is where a value gets a name". The load-bearing half is
"not a layering question at all" -- that is what stops the block being weighed against the three
questions. The destination half is an example that generalized.

It holds for the rail block, on a reason the sentence does not give: `--rail-width` and
`--rail-column` are a cross-language contract that `apps/local/src/i18n/width.rs` derives two
constants from by hand, and `@monoflake/sdk` is this repository's established answer for a value two
languages need. It is untested for a block that is genuinely one site's and crosses no boundary,
where `apps/site/src/styles/` would be the better home.

**The contract is cited four times, in both directions, and nothing checks any of them.** The
reference check validates markdown links, `spec/**/*.md` cited from code, and quoted section names;
a path written inside a Rust doc comment or a CSS comment is in its not-flagged set, whichever end
it points at. The four, with what each names:

- web's `apps/local/src/i18n/width.rs`:253 gives `--rail-column` and the path
  `apps/site/src/styles/utilities.css`, deriving `DESKTOP_TITLE` and `DESKTOP_SUBTITLE` from it.
- web's `apps/local/src/i18n/width.rs`:12-17, above `ONE_LINE`, gives `--rail-width` and says
  "Move that declaration and move this" -- with **no path at all**. There is nothing here for a
  check to validate even if one read Rust comments, and nothing for a reader to follow either.
- web's `apps/site/src/styles/utilities.css`:84, in the `--rail-width` comment, gives
  `ONE_LINE` and the path `apps/local/src/i18n/width.rs`.
- web's `apps/site/src/styles/utilities.css`:88, in the `--rail-column` comment, gives
  `budget::DESKTOP_TITLE`, `DESKTOP_SUBTITLE` and the same path.

**Two of the four carry something that is checked, and they are the same two.** The `--rail-width`
pair -- `width.rs`:12-17 and `utilities.css`:84 -- each cite web's `spec/styling/rail.md`,
"The rail's box is one declared width", and a quoted section name is checked. The `--rail-column`
pair cite no spec section at all, so nothing in either comment is verified by anything. The half
that holds itself up is the pointer to the argument, never the pointer to the other language.

Whoever moves the block updates three paths and re-reads a fourth comment naming a property with no
file beside it, and nothing will say they forgot any of the four.
