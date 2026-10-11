---
name: Planning in RFCs
title: What is not done is an RFC with a status, blocked by others by id, and a spec says only what holds now
status: landed
supersedes: [qp9l1]
commits:
  [
    web@eed91b9c,
    rfcs@3025ea23,
    workspace@7dc6bd7a,
    workspace@ff606cc8,
    web@89464977,
    platform@455f70ff,
    infra@53188eee,
    lib@198fb556,
    seam@5c7ac9a0,
  ]
references: []
---

## Question

RFC qp9l1 let an open question's argument be a draft, still cited by a line in `issues.md`. A
decided piece of work waiting its turn stayed a `todo.md` entry, and what waited on what was prose,
where it was written at all. Every repository kept three planning files beside its rules, an entry
in two places once it had a draft. Can the RFCs hold all of it, so that a `spec/` says only what
holds now and the order of the work is read off a graph rather than kept by hand?

## Options

| Where what is not done lives                   | Against it                                                              |
| ---------------------------------------------- | ----------------------------------------------------------------------- |
| `issues.md` and `todo.md`, each citing a draft | Every entry twice, and the order of the work prose unchecked            |
| RFCs alone, each with a status, blocked by id  | One more repository to read for what is left, and a graph to hold whole |

## Decision

RFCs alone.

- **Every RFC has a status**, and each holds its body and its commits to a form:

  | `status`   | Means                  | `## Decision` | `commits`    | Edited |
  | ---------- | ---------------------- | ------------- | ------------ | ------ |
  | `draft`    | the question is open   | none          | none         | yes    |
  | `accepted` | decided, not yet built | written       | none         | yes    |
  | `landed`   | built and pushed       | written       | at least one | never  |
  | `declined` | decided against        | written       | none         | never  |

- **A landed RFC names at least one commit, and each is checked to exist** in its repository where
  that is cloned, `workspace` naming the workspace itself. An RFC built in the commit that files it
  is `accepted` there and `landed` in the next, which can name the first.
- **`blocked-by` names the RFCs that must land first, by id and nothing else.** A wait on anything
  outside, a vendor or a machine, is written in the body. The graph has no cycle; an RFC lands only
  once each it is blocked by has landed or been declined; an open RFC is never blocked by a declined
  one. What an RFC blocks is read off the others, never written.
- **What to do next is worked out, not written down**: `mise run plan` in `rfcs` lists the open
  RFCs nothing open blocks, and the chain behind each of the rest.
- **A `spec/` holds what holds now.** `issues.md` and `todo.md` leave every repository, an issue
  becoming a draft and a todo entry an accepted RFC, each filed on the day it was first written as
  near as the history shows. A milestone is an accepted RFC too, the milestones chained by
  `blocked-by`, and a finding is one RFC however small, so each can be blocked or declined alone.
  `roadmap.md` stays, a few lines of direction and nothing to do.
- **A code comment cites an RFC by id**, as a rule does, wherever it cited an issue or a todo entry.
- **cue and still file no RFCs for now**: cue is private, and still has no spec. Their planning
  stays in their own `spec/` until either changes.
- **The author's own public names may be written**: `canmi.net`, `canmi.app`, `canmi.icu`,
  `monoflake.com`, `monoflake.net`, `ffoni.com`, `ixc.one`, `ill.li` and `symlink.si`, and the hosts
  under them. Any other host outside `references` is still a reserved one; in code, where
  `stylex.create` is a call, only a name ending as a host commonly does is read as one.
- **Only a landed RFC supersedes**, and only a landed one is superseded; an open RFC may name what
  it would.
- **The RFCs filed before this one take `status: landed` and the commits they landed in**, the one
  edit a frozen RFC is given, since neither field held it before.
