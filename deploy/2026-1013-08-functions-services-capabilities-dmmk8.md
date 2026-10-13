---
name: 'Functions Services Capabilities'
title: 'Compute is a function, a sleeping service or a resident one, and what only a server can do is offered to functions as a capability through a binding'
status: accepted
supersedes: []
blocked-by: [l4s6a]
commits: []
references:
  - url: https://developers.cloudflare.com/browser-rendering/
    took: 'a headless browser kept by the platform and handed to a Worker as a binding, never started by it'
  - url: https://developers.cloudflare.com/containers/
    took: 'a container a Worker starts on demand, which sleeps once idle'
  - url: https://knative.dev/docs/serving/
    took: 'an activator that holds the first request while a service scaled to nothing starts'
---

## Question

A function runs in an isolate with fixed limits -- RFC l4s6a -- but some of what the platform runs
cannot: `shot` drives Chromium in two gigabytes, and `geo` maps GeoLite2 and GeoNames from disk. The
author asked on 2026-10-13 where the line between a function and a server is, how such a service
fits a serverless platform, and whether a third form, a process of its own that sleeps when idle,
should exist between the two.

## Decision

Decided with the author on 2026-10-13.

- **Compute has three forms, and two kinds underneath.** A **function** is an isolate per call, RFC
  l4s6a. A **service** is a container of its own, in any language. A service with `min_instances =
0` **sleeps** when idle; one with `min_instances` of one or more is **resident**. A sleeping
  service is not a third runtime but a service's setting, so host learns to freeze, stop and wake
  rather than to keep a third kind of environment.
- **Something is a service, never a function, if any one of these holds**: it runs a native process
  (Chromium, ffmpeg); it keeps warm state between calls that is costly to rebuild (a browser, a large
  index, a model's weights); its data is larger than a function's memory, or is mapped from disk;
  or it cannot start within a function's 1 s. Otherwise it is a function.
- **A sleeping service sleeps in two depths.** First frozen, by the cgroup freezer (`docker pause`):
  no CPU, its memory left for the node to swap out, woken in milliseconds with its warm state. Then,
  idle longer, stopped: its memory freed, woken in seconds.
- **host holds the first request while a service wakes**: Caddy hands a request for a sleeping
  service to host, which thaws or starts it, waits for its health, then passes the request on, or
  answers `503` past a deadline.
- **What only a server can do is offered to functions as a capability, through a binding**, as
  Cloudflare offers its browser, images and AI. A caller sees a binding, whichever of the three forms
  answers it, and the form is a placement the platform chooses and the console changes, never a
  change to the caller's code. So the platform is servers, and what is built on it is serverless.
- **`shot` splits in two.** A `browser` capability: a pool of Chromium kept warm, resident, on `tyo`
  and `rdu`, answering a render with bytes, and reusable for a PDF or a page read. And `shot`'s own
  work as a function: a capture is a ledger task (RFC 23wgz), its render a call to the binding, its
  pictures written to a bucket by their content, whose lifecycle replaces today's four gigabytes on
  disk.
- **`geo` stays a service**: mapping its data is what keeps it to 64 MB and instant to start, and an
  isolate can map nothing. It may sleep, since a stopped `geo` starts on a page cache that is still
  warm. A caller's own address needs no `geo` at all: the edge already says where it is.
