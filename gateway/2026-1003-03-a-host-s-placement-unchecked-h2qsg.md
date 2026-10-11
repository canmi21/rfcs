---
name: "A Host's Placement Unchecked"
title: "A deployment's host is not checked against where its service runs, and choosing among placements is open"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`geo-glo-cf.ixc.one` reads as geo on Cloudflare's Workers, and the gateway sends it to the node at
home, where geo runs, all the same: a profile's placement is read and not checked against the
service's declared placements, since every service has one placement today. Refusing a placement a
service is not at, and choosing among several where it is, come together with the gateway's choice
of where a request runs.
