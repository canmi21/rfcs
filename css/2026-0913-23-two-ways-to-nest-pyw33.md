---
name: Two Ways to Nest
title: 'A declaration can nest two ways, and a sweep that reads one level finds neither'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Three declarations kept their literals through the value-naming step, all in `chainMarker` on the
package route: `borderRadius: 'calc(infinity * 1px)'` and `borderWidth: '1px'` under `'::before'`,
and `borderLeftWidth: '1px'` under `'::after'`. Each has a name -- `radius.full` and
`border.hairlinePx` -- and they are exactly the three declarations in that key which do. The other
five are two `solid` keywords and three token colors, which the vocabulary never held.

The cause is a property of the source rather than of whoever swept it. A style key's value is
either a declaration or an object, and it is an object in two unrelated cases: a **conditional
value**, whose keys are `default`, a pseudo-class or an at-rule, and a **pseudo-element block**,
whose key is `'::before'` and under which everything is a declaration again. The two mean nothing
alike and nest identically, so a reader that takes a key's value to be a declaration misses both,
and misses them silently -- a literal that was never visited looks exactly like one that was
visited and left.

The counts say how narrow this instance was and how wide the shape is. Ten declarations across the
site sit under a pseudo-element block, in three keys: `chainMarker`, and the `::placeholder`
colors in web's `apps/site/src/lib/newsletter/newsletter.svelte`'s field and
web's `apps/site/src/lib/search/dialog.svelte`'s query. The other two hold token
colors only, so three was the whole of the miss. Conditional values are much the larger half and
were not missed, which is luck rather than design -- the value-naming step happened to read them.

**Nothing in the tree makes the next sweep walk to the leaves.** The entry on the extraction
threshold wants a count taken across three layers; the entry on whether a declaration moved wants a
committed list of what the stylesheet declares. Both are sweeps, both meet this, and a sweep that
under-reports is the one kind that reports success. What it would cost to fix is one shared reader
that walks a style object to its leaves and yields paths rather than keys. What it costs to leave
is invisible every time, which is the argument for writing it down rather than remembering it.
