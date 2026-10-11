---
name: Hand-Written Whitelists
title: "The firewall's whitelists are written by hand and checked against nothing, and the new zones have none"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`rules/ill.li/alias-paths-only.txt` spells its host's paths out by hand, and nothing checks it
against the gateway's table. The new zones -- `monoflake.com`, `monoflake.net`, `ixc.one`,
`symlink.si` -- carry the shared rate cap and their redirects, and no whitelist yet.

## Evidence

A generated whitelist has two limits to fit inside: an expression is at most 4,096 characters, and
the Free plan allows a zone five custom rules. Every deployment of `ixc.one` is in one zone, so its
whitelist covers every service on every node in one expression.
