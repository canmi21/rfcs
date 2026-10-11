---
name: 'Attribute Conditions via :is()'
title: 'An attribute condition is one `:is()` away from the visual layer after all'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The entry above measures that StyleX's conditions are pseudo-classes, pseudo-elements and at-rules,
and concludes that an attribute-conditioned value has nowhere in the visual layer to go. This entry
opened by quoting a stronger sentence that has since been retracted, so the quote is withdrawn from
here too. web's `spec/architecture/css/authoring.md` now calls the impossibility
"a true observation about the type generalized one step past what had been tested", and carries the
same compiled rule this entry does. The type in question, `` `:${string}` ``, is exactly the hole:
StyleX rejects a key by what it opens with rather than by what it contains, and
`:is([data-highlighted])` opens with a colon. Compiled through the same Babel plugin the build
uses:

```css
.xbhnonm:is([data-highlighted]) {
	background-color: var(--color-paper-hover);
}
```

Nothing on the site writes one, which is why the language switcher's menu rows kept
`data-[highlighted]:bg-paper-hover` in the markup along with the mark's `text-text-soft` and the
`group-data-[highlighted]:text-text-strong` it cannot be separated from. Choosing `:is()` mid
migration would be inventing a convention rather than moving a declaration, and the ancestor half
still needs the marker that the ancestor entry above is holding.

So this does not settle that entry, it widens it. The choice is not two ways but three: the markup
keeps every attribute-conditioned value, or the selector layer does, or `:is([attr])` becomes how
the visual layer says an attribute and the same spelling is used everywhere. Whichever is chosen,
the sentence in web's `spec/architecture/css/authoring.md` has to change with it: a rule stated as
an impossibility is the one kind a reader never re-measures.

That sentence was changed before any of the three was chosen, which is the convergence the top of
this entry now records: each file carries the counterexample and cites the other. The choice above
is still open, and this entry stays with it.
