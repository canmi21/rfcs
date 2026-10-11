---
name: Body Typography Reaches Far
title: "The article body's typography reaches elements no component renders"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

61 of the 351 rules in Svelte `<style>` blocks were `:global` at the time, and the largest group is
web's `libs/prose/src/prose-root.svelte` and
web's `libs/prose/src/body.svelte` styling prose the markdown compiler
produced: `strong`, `s`, `hr`, `blockquote`, `pre`, `code`, `picture`, `img`, `.shiki span`.

Under the three layers this is correctly the selector layer, because there is no element to put a
class on. What is unresolved is that it is also the site's prose typography, which is the visual
layer's subject, and it is currently unreachable from it. Whether the compiler should be emitting
classes the visual layer can name is the decision, and it reaches into the content pipeline rather
than into a stylesheet.
