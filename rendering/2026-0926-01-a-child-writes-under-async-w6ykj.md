---
name: A Child Writes Under Async
title: "Under the async render a child's write into an object its caller reads lands before the caller's read, unlike Svelte's, and which fix is open"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

**A child's write into an object the caller reads, under the async render.** Not yet measured by
the suite, and owed. `runtime-legacy/binding-backflow`'s `reactive_mutate` and `init_mutate` cases
have the child do `value.foo = 'kid'` on the object its caller passed as `value`; `bind_props`
sends nothing up, since the caller's value is not `undefined`, so Svelte writes the caller's
`{value?.foo}` before the child runs and it reads `mon`, in both renders. The synchronous pass
agrees. Under `experimental.async` this compiler writes `kid`: the caller's read is an async
derivation and the child is a held run, and the run is evaluated before the derivation reads, so
the write lands first. It stays out of the suite's colour only because upstream never renders the
legacy suite async and the runes corpus has no sample of this shape; a runes project with the flag
on and a child that mutates a prop object would meet it. It is "Shared mutable state a function
reaches" in seam's `spec/readings.md`, met through evaluation order rather than through a name.
Two answers, and the choice is not made: evaluate the async derivations of a structure in source
order, which is Svelte's order and costs the concurrency the async build has; or hand a held run a
copy of what it is passed, which is not Svelte's semantics -- the object is one object there, the
read is just earlier. The first is the one to try; the order the program evaluates what waits in
(`evaluated()` in `pkgs/program/src/derivations.ts`) is where it starts. Kit's `async` and `options-2` apps turn the flag on, so stage two is where a
test for it arrives; it is taken there rather than ahead of it.
