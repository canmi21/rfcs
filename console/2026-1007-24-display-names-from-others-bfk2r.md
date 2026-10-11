---
name: Display Names From Others
title: Where display names come from once others deploy apps
status: draft
supersedes: []
blocked-by: [nzscl]
commits: []
references: []
---

## Question

The console's copy of every display name -- web's `spec/architecture/console.md`
-- holds while every app is the author's and a test can read every declaration. An app a friend
deploys from their own repository is in no checkout beside this one, so its name has to reach the
console at run time: relay carrying `display_name` from host's apps, or a registry the platform
keeps of every app it deploys, Workers included. Undecided until accounts exist.
