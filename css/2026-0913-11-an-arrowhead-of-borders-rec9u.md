---
name: An Arrowhead of Borders
title: 'An arrowhead is a shape made of borders, and the test cannot cut it in half'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `libs/prose/src/blocks/quadrant.svelte` tips each of its two axes with a
triangle, drawn the way CSS has always drawn one: a pseudo-element at `width: 0; height: 0` with
three borders, two of them transparent and the third the arrowhead itself.

```css
.vertical-rule::before {
	width: 0;
	height: 0;
	border-inline: 0.3125rem solid transparent;
	border-block-end: 0.5625rem solid var(--color-border-strong);
	content: '';
}
```

Every test in web's `spec/architecture/css/layers.md` gives two answers here at once. The border
widths are the only size the element has, so they decide how large it is; the border colors decide
how it looks, and one of them is a token. `transparent` is neither: on a box with no width it is
how CSS says a side does not exist, which is shape rather than appearance. And `content` is not
classifiable at all -- it is what brings the element into being, so the rule survives whatever else
leaves it.

The rule stayed whole, and the cost of that is worth naming rather than leaving implicit. The axis
line beside the arrowhead is `border-inline-start: 0.0625rem solid var(--color-border-strong)`,
which did move, so one figure's ink is now written in two layers from the same token. Changing the
axis to a different one leaves the arrowhead behind, which is the trap the keyframe entry above
describes in a different property. StyleX can reach a pseudo-element -- the newsletter already
writes `'::placeholder'` -- so the split is available; what stops it is that taking it means
rewriting the shorthand into longhands in the selector layer to make room for the color, which is
restructuring rather than moving.

Deciding it is either that a shape drawn out of borders is one declaration however many properties
it spans, or that the arrowhead stops being borders -- a `clip-path`, or a glyph -- and the
question disappears with the technique.
