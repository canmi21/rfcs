---
name: A Reduced-Motion Answer
title: A reduced-motion answer is three declarations that only mean anything together
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Nine style objects, across eight components, write the same three lines against
`@media (prefers-reduced-motion: reduce)`: `transitionProperty: 'none'`, `transitionDuration:
'0s'` and `transitionTimingFunction: 'ease'`. None of the three was given a name. Each is a CSS
keyword or a zero, so a name would replace a word a reader already knows with a word they would
have to look up, and `0s` is the value least worth a lookup on the site.

The repetition is real anyway, and it is not of a value. What repeats is a _set_: three
declarations that say one thing, which is that this element does not animate for a reader who
asked for that. The three do not always travel together: counted across the same blocks the
property appears twelve times, the duration fourteen and the curve nine, and
web's `apps/site/src/lib/locale/switcher.svelte` answers with the property alone.
Whether each of those is deliberate is a question one name would have made visible and three
literals never will. `defineConsts` cannot hold it -- a const is a value, and this is three of them
against a condition. `stylex.create` cannot hold it either, and this entry said it could.

**What stood here was that it can**, "and a shared style composed into each component is what the
visual layer is for, which is the argument web's `spec/architecture/css/layers.md` makes for the
layer in the first place". The argument is sound and the premise is false, which is the combination
worth leaving on the page: a shared composed style is exactly what the visual layer is for, and
this set is the one kind of thing it cannot be made of.

Measured on the pinned Babel plugin. A compiled style object maps one key per property to one class
string, conditions included, so a group carrying only the reduced-motion branch of
`transitionProperty` is not a branch added to what the component declares -- it is replaced by it.
Against a card writing the three defaults, `props(group, card)` comes back with the three default
classes and none of the three branch classes; reversing the arguments loses the defaults instead.

**The stylesheet cannot report that**, which is why the reading looked safe. Both forms compile the
same six classes, because the group's declarations are still compiled wherever they are written.
Only the element's class attribute says which of them it ended up wearing.

So the branch travels with a default, and the nine sites have nine of them: the two-item lists on
github's card and twitter's, which are a copy of each other, `transform, translate, scale, rotate`
on the code block's chevron, `opacity, transform` on its copy icons, `transform` on the enlarged
picture, `grid-template-rows` on the article summary, three-item lists on the support pill, and
`color` and `transform` on the two footnote controls. The escapes were measured and none is
available: a function in `create` emits `var()` for each default and registers three custom
properties, which is six rules the stylesheet did not have; a helper called inside `create` fails
the build on the cross-file path web's `spec/architecture/css/extraction.md` already records as
broken; and there is no `include` in the API at 0.19.

What is left to name is the three literals, which the paragraph above declines. The composed style
this entry was waiting on now exists -- surfaces.ts in `@canmi/kit` holds four
groups -- and this is the one thing it cannot hold. So the entry is no longer about naming and is
entirely about the reading: whether
web's `apps/site/src/lib/locale/switcher.svelte` answering with the property alone,
and the twelve, fourteen and nine counts above, are that many deliberate decisions or one decision
written inconsistently. No arrangement of the visual layer will make that visible, and answering it
is a person reading nine style objects.
