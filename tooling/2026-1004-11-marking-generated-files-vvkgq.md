---
name: Marking Generated Files
title: '`.gitattributes` is a list only a long file is held to'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The paths marked `linguist-generated=true` keep a forge reading this repository as what somebody
wrote, and since the workspace's line check began measuring every file, they are also what it
passes over -- the workspace's `spec/architecture/repos.md`, "Machine output is
marked, so the language statistics describe the repository". A generated file over the hard limit
that nobody marked now fails the gate, which is how `data/record/metadata.json`, marked under a
path it had left, was found. One under the limit still goes on being counted and nothing fails.

Deciding the rest costs a definition: a gate that answers "is this machine output" without a
person needs the set of paths the generators write, a second list kept by hand. Worth doing only
alongside a reason to enumerate those outputs anyway.
