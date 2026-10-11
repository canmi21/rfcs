---
name: A Changed Env Needs a New Host
title: 'A changed .env reaches host only when host is replaced, and how the node task gets it read is open'
status: landed
supersedes: []
blocked-by: []
commits: [infra@2168b7c9]
references: []
---

## Question

`mise run node` rewrites host's `.env` -- a slot, a grant -- and says host needs recreating, but
only keeper replaces host, and only for a run that built it. On 2026-10-07 every node's host was
made to read its new `.env` by sending each the last infra run again by hand, which keeper took as a
replacement of host by the same image.

## Options

| How host comes to read a changed `.env`                  | Against it                              |
| -------------------------------------------------------- | --------------------------------------- |
| The node task asks keeper to replace host itself         | A deploy's machinery used for a setting |
| keeper gains a route replacing host by the image it runs | One more door on keeper                 |
| host reads its grants from a file it watches             | A second way a setting reaches host     |

## Decision

keeper gains a route replacing host by the image it runs.

- **keeper recreates host from the version and image it runs**, on the node's `.env` as it now is,
  and puts the old one back if the new one fails.
- **`mise run node` asks it to** whenever it changed host's `.env`, as `recreate-host` does by hand.
