---
name: "The CMS's Own Style"
title: 'The CMS keeps a style of its own, and moving it waits on the platform'
status: draft
supersedes: []
blocked-by: [nzscl, l4gr0]
commits: []
references: []
---

## Question

The CMS made a style unlike the site's, and is to move to the console's neutral one --
web's `spec/architecture/space.md`, "The design system: its layers, and a
style across them". It waits in a chain: the design system comes first, the platform's interface
is built on it, the platform is waiting on that interface, and the CMS's move waits on the
platform -- RFC nzscl and RFC l4gr0.

**What deciding it would cost.** The move and a check for visual regression, once the platform
it waits on is there.
