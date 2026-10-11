---
name: A Shadow Beyond Restating
title: 'A shadow is one utility, two declarations and four variables the visual layer cannot restate'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Tailwind 4.3.3 compiles `shadow-lg` to a rule holding two declarations, not one:

```css
.shadow-lg {
	--tw-shadow:
		0 10px 15px -3px var(--tw-shadow-color, rgb(0 0 0 / 0.1)),
		0 4px 6px -4px var(--tw-shadow-color, rgb(0 0 0 / 0.1));
	box-shadow:
		var(--tw-inset-shadow), var(--tw-inset-ring-shadow), var(--tw-ring-offset-shadow),
		var(--tw-ring-shadow), var(--tw-shadow);
}
```

The four ring variables are registered with `@property` at an initial value of `0 0 #0000`, so the
computed shadow on an element carrying the utility is six layers, four of them transparent. A
`boxShadow` in the visual layer holding only the two real ones renders the same pixel for pixel and
computes differently, which is the one thing a migration is not allowed to do.

There are three ways to write it and none of them is good. Declaring `--tw-shadow` from StyleX and
composing the same five-term `box-shadow` puts another framework's private variables in our source
and depends on registrations Tailwind emits only while some shadow utility survives somewhere on
the site. Spelling the four placeholders out as literals -- `0 0 #0000, 0 0 #0000, 0 0 #0000,
0 0 #0000,` and then the real pair -- computes identically and says nothing to a reader about why
four transparent shadows are there. Dropping them changes the computed value.

So `shadow-lg` on web's `apps/site/src/lib/search/dialog.svelte`'s panel and
`shadow-sm` on web's `libs/prose/src/blocks/cargo/cargo.svelte`'s tooltip stayed in
the markup, under the set rule in web's `spec/architecture/css/migration.md`: Tailwind writes the
variable and the shorthand as a unit in one rule, which is checkable, and the member that cannot
move is the `@property` registration. Four more sites carry the same utility --
web's `apps/site/src/lib/components/modal.svelte`,
web's `libs/prose/src/components/popover-content.svelte`,
web's `apps/site/src/lib/components/menu-content.svelte` and
web's `libs/prose/src/blocks/tokei/tokei.svelte` -- so the decision is the site's
rather than one component's.

It is the same shape as `transition-colors`'s three `--tw-gradient-*` variables, one step worse.
There the private names sit inside a value the visual layer can still state in full; here the value
is composed out of registrations the visual layer has no way to make. Deciding it is either a
shadow of our own in the token layer, which is where a surface's elevation arguably belonged all
along, or saying that a utility whose value is assembled from registered variables is not a
utility the visual layer takes.

The migrated form was written and reverted on
web's `apps/site/src/lib/components/menu-content.svelte`, which is where the cost
showed itself. A `--tw-shadow` declared from StyleX beside the five-term chain compiles and renders
correctly today, and it is correct only while five other components keep a class this one does not
control: the registrations reach the document because Tailwind scans the markup and finds a
`shadow-` utility somewhere in it. Migrate the last of them and `--tw-inset-shadow` is never
registered, an unresolved custom property makes the whole declaration invalid at computed-value
time, and this panel's `box-shadow` becomes `none`. There is no build error and no console warning,
and the gate cannot see it either: a menu renders nothing until it is opened, which
web's `spec/architecture/css/migration.md` already lists among the surfaces a snapshot never
reaches. A component whose appearance depends on another component keeping a class is a worse
arrangement than the one it replaced, and the failure arrives in a file nobody was editing.

So this is a third reason a declaration cannot leave the layer it is in, and it is neither of the
two already in this file. A rule stays in the selector layer because it needs a selector the other
two layers have no way to write. An SVG presentation attribute stays below every layer because
moving it would change which layer wins. Here the declaration cannot be written by anybody in any
layer: an `@property` registration is not something a component emits, and without it the value the
visual layer would have to state does not resolve at all.
