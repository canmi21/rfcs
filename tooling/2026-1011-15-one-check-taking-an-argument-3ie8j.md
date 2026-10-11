---
name: One Check Taking an Argument
title: 'The `check-*` gates might be one `check` taking what to check'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`new-rfc`, the rfcs repository's, and web's `check-*` gates are each a verb and its object joined
by a hyphen, while `new rfc <scope> <slug>` and `check rfc` read as the action they are. Making the
object an argument means `check`, today the whole-repository type check taking nothing, becomes a
dispatcher over every `check-*`, and `verify`'s gate table names them anew; the workspace's own
`check <name>` names a repository, not a gate, so the two would read alike and mean different
things. Worth deciding with any other reason to rename the gates.
