---
name: 'Buckets Over Stores'
title: "A bucket is the platform's abstraction an app sees, a store is where bytes are, and both are resources the console holds with their limits"
status: accepted
supersedes: []
blocked-by: [cpc2b]
commits: []
references:
  - url: https://developers.cloudflare.com/r2/pricing/
    took: 'storage counted as the bytes of the objects kept, which is what a bucket is limited by here'
---

## Question

The platform's storage was written down as a bucket over stores -- platform's
`spec/architecture/scheduling.md` -- but neither is anything the platform holds. A provider's bucket,
or a node's disk, is known only to whoever set it up, and has no limit set anywhere. Meanwhile
`objects.md` gives every app its own S3 sidecar over its own directory, with no index and no content
address. The author asked on 2026-10-12 what is exposed first, how a limit is set, and whether an
app should see names or content addresses.

## Decision

- **The words are fixed.** A **bucket** is the abstraction the platform built, the one an app sees.
  A **store** is where bytes are kept: a provider's bucket, or a share of a node's disk. The
  platform is the layer between them.
- **Both are resources in the registry**, of kind `store` and `bucket`, in RFC cpc2b's shape, so the
  console shows them and writes them.
  - **A store's `spec`**: its provider, endpoint and credentials, the credentials named and never
    kept there. Its failure domain, and whether it is redundant by itself. Or the node it is on.
    Its `capacity`, and a `reserve` kept free below it: a provider's 10 GB written to 9.5 GB is a
    capacity of 10 GB with a reserve of 0.5 GB.
  - **A bucket's `spec`**: its scope, its `quota`, its layout of stores, and its access class.
  - **A store's `status` is what is used on it**, which a controller reports.
- **The two are counted apart.**
  - **A bucket's quota counts logical bytes**: the size of every key it holds, each counted, however
    many share the same content and however many copies are kept. An app reads it as it would a
    provider's bill.
  - **A store's capacity counts physical bytes**: each content address it holds counted once. On a
    node's disk, this is also measured against the filesystem, the larger of the two taken. The
    filesystem's own overhead, such as block alignment, is what the reserve absorbs.
- **A node's disk is divided by limits the node declares**: its stores, each a btrfs subvolume under
  a quota group, and the room container images may take. A node may hold one store or more.
- **A store is content-addressed and knows nothing else.** It keeps bytes at
  `{scope}/{ab}/{cd}/{cid}`. Stores of any provider can be mixed in one layout, since none holds a
  name.
- **An app sees names, and content addresses when it wants them.** A put answers the content
  address as its ETag. An object can be read by its address, and an app can ask whether an address
  is held before uploading the bytes. The platform hashes what is written and never trusts a hash it
  is given.
- **What a name points to is the platform's, never the app's**, because whether bytes may be
  reclaimed depends on every reference in the scope. A scope's index is held by that scope's bucket
  service, the one instance that writes it, as an embedded SQLite -- RFC kloyq. Its WAL is shipped
  to stores directly, never through the bucket it indexes.
- **`objects.md`'s sidecar becomes a store**: Versity over a node's disk, one kind of store among
  several, behind the bucket service rather than in front of an app -- RFC cg93k. No app uses it
  yet, so nothing moves.
- **A bucket's layout changes while it serves**, decided with the author on 2026-10-13. A layout
  written from the console is what is wanted, and a background job copies each content address onto
  the stores the new layout names, checks it by its hash, and only then records it there. Reads come
  from any store holding the address meanwhile, and new writes follow the new layout. An old copy is
  let go only once the new layout is met, and then by its store's own way of letting go. Since bytes
  never change under an address, no copy is ever half-old, and the job is the scrub's, level-triggered.
- **The order**: the two kinds come first, so what the platform has, and what each limit is, can be
  seen and set; the bucket service, RFC pxp3l, follows.
