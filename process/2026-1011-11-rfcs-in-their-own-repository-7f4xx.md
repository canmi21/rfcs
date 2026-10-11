---
name: RFCs in Their Own Repository
title: The RFCs leave web for rfcs, a repository of documents alone, which web's code checks and draws by path
status: landed
supersedes: [2ut6h]
commits: [rfcs@df5bc9f8, web@e709cb3b, workspace@aa87cc3d]
references:
  - url: https://github.com/rust-lang/rfcs
    took: a series' repository named for what it holds, in the plural
  - url: https://github.com/python/peps
    took: the same, for another series
---

## Question

The RFCs were filed under web's `rfcs/` so that their pages, being tried while the design system
was, could change in the same commit as the files they draw. Their pages are not being drawn now;
the areas they cover, the process and the design system, are followed by every repository; and
lib's rules cite them already, a rule in one repository pointing at a record kept in another. Do
the RFCs stay in web?

## Options

| Where the RFCs live                              | Against it                                                                |
| ------------------------------------------------ | ------------------------------------------------------------------------- |
| web's `rfcs/`, as now                            | Every repository's record kept inside one of them, its history with web's |
| A repository of their own, read by path from web | Two checkouts to build the pages, a sibling the workspace already gives   |

What the repository is called, the rest of the workspace's names being singular:

| Name   | For it                                                                          | Against it                                         |
| ------ | ------------------------------------------------------------------------------- | -------------------------------------------------- |
| `rfc`  | Reads alongside `lib` and `web`                                                 | Says one request for comments, where it holds many |
| `rfcs` | Says what it holds, as `/rfcs` does and as RFC 2ut6h's plural `rfcs/` was given | The one name in `repos.toml` ending in an s        |

`lib` and `web` are no precedent for the singular: a library is one collection of many packages,
and the web is a domain, neither counted, where an RFC is one document and a repository holds many;
the other names are domains or proper names as well. `rfcs`, then, as other series' repositories
are named. `new-rfc` and `check-rfc` stay singular, since each acts on one.

Where the code that checks them lives, the draft having moved it with the files:

| The format's check, `new-rfc` and the schema | Against it                                                    |
| -------------------------------------------- | ------------------------------------------------------------- |
| In `rfcs`, beside the files                  | Code in a repository of prose, under a license made for prose |
| In web, beside the pages, run on the sibling | `rfcs` verifies only with web cloned beside it                |

## Decision

A repository of their own, `rfcs`, holding documents alone.

- **The RFCs are the `rfcs` repository, their areas at its root**:
  `{scope}/{yyyy}-{mmdd}-{nn}-{slug}-{id}.md` beside `scopes.yaml`, where RFC 2ut6h filed them
  under web's `rfcs/`.
- **It holds documents and the few settings that check them, nothing else.** The check, `new-rfc`
  and the schema stay in web beside the pages; the repository's `verify` and `new-rfc` run them on
  its own checkout, so web is cloned beside it.
- **It is licensed CC BY-NC-SA 4.0**, the one repository not MIT, since it holds no source.
- **The pages stay in web, still unpublished**, reading the files from the sibling checkout.
- **That a rule cites its RFC by id is the workspace's rule**, since every repository follows it.
- **It starts from the files as they were**; their filing's history stays in web's.
