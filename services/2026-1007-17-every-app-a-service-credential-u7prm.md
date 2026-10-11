---
name: Every App a Service Credential
title: "Every app is given a service credential when it is deployed, the first part of the platform's identity"
status: accepted
supersedes: []
blocked-by: [z9vqb]
commits: []
references: []
---

## Question

Who asks is a principal, and an issuer vouches for it -- platform's `spec/architecture/scheduling.md`,
"Who asks is a principal, and an issuer vouches for it". Nothing issues an app one yet.

## Decision

- **Every app is given a `service:<app>` credential when it is deployed**: the platform's identity,
  first part, third of the services that keep nothing.
