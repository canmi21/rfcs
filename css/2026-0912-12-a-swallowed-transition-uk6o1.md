---
name: A Swallowed Transition
title: An unlayered utility swallows a transition the markup still carries
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The newsletter's unsubscribe control is `focus-link spring-underline` and, until this migration
moved it into the visual layer, `transition-colors duration-200` beside them. Measured, the control
reported `transition-property: --underline-progress`, a duration of 315ms and the shared spring:
`.spring-underline` in web's `apps/site/src/styles/utilities.css` sat outside every
`@layer`, an unlayered rule outranks every layered one, and its `transition` shorthand takes all
four longhands. The ten-property list never reached the element, and the hover color on that
control snapped rather than faded. Moving the declaration changed nothing, because StyleX is
layered too and lost to the same rule -- which is why it was carried across unchanged rather than
dropped, a migration moving what the markup said rather than what it achieved.

**The heading is wrong in its first word now, and the mechanism inverted rather than went away.**
`.spring-underline` is in `@layer components` at
web's `apps/site/src/styles/utilities.css`:210. StyleX's priority layers sit above
Tailwind's `components`, so a `transition-*` in the visual layer on that element now beats the
shorthand instead of losing to it. That is the reversal
web's `spec/architecture/css/layers.md` predicted under "`utilities.css` is
dissolved, not given a position", which names this control among the three to expect rather than
discover.

**What the control does is unchanged, and it is now held on purpose.** No markup on the site carries
`transition-colors` beside `spring-underline` any more -- counted, zero sites -- and the two
components that could now declare one in the visual layer decline to, each with a comment saying
why: `undo` in web's `apps/site/src/lib/newsletter/newsletter.svelte` writes no
transition at all, so the hover snaps and the underline springs, and `fade` in
web's `apps/site/src/lib/error/offer.svelte` is confined to the form's button for the
same reason. The swallowing is gone and the snap it produced is deliberate. What holds it is a
comment at each call site, which is weaker than a check and stronger than the accident it replaced.

**What is still open is the general half, and it has lost the half that made it general.**
`@canmi/ui/primitives` declares no `@layer` at all, so the named
layer is unlayered there and layered in `utilities.css` with nothing saying which a given rule
should be, and an element carrying a primitives class and a utility for the same property still has
no way to say which it meant. The deciding sentence here was giving `utilities.css` and
`@canmi/ui/primitives` a layer of their own; `utilities.css` took Tailwind's `components` instead, which
answered the cascade and settled nothing about the boundary. What is left is `@canmi/ui/primitives`
alone, and it is the first entry in this file's question rather than this one's.
