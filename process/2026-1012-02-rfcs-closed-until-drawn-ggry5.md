---
name: 'RFCs Closed Until Drawn'
title: The RFCs' pages are reached nowhere, development included, until they are drawn, and cited by id as words
status: landed
supersedes: [43im3]
blocked-by: []
commits: [web@a3530fbb]
references: []
---

## Question

The RFCs' pages are read in development alone, and a citation of one is a link there. With no UI
drawn for them yet, should development reach them at all?

## Options

- Keep them reached in development, the citation a link there.
- Close them in development too, the citation the id as words everywhere, the files read where they
  are filed.

## Evidence

Reached in development, the pages stood in the corner's menu and under every citation, a draft of a
UI nobody had drawn, opened and judged as if it were one. The RFCs are read as files meanwhile,
found by id at `repos/rfcs/*/*-<id>.md`, and their check, their new-rfc and their plan run from the
app's scripts with no server.

## Decision

- **A part not yet published reaches either development alone or nowhere**, and the RFCs reach
  nowhere: `/rfcs` answers 404 in development as in production, the corner's menu leaves them out,
  and development does not start their server.
- **A citation is the id as words everywhere**, `RFC 2ut6h`, until the pages are drawn.
- **Both lift together** once the RFCs' pages are drawn by the settled system: the part loses its
  mark, and every citation becomes a link.
