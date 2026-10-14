---
name: 'The Relay Sends What Changed'
title: "The relay sends a node's snapshot once and then only what changed, compressed, its readings on a beat over UDP, and its events not at all"
status: accepted
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The meter counted relay at 13 to 21 GB a day on every node, about 134 GB a day across nine, on
2026-10-14. Measured on `rdu`: a snapshot is 18 to 23 KB, 80 to 85% of it host's newest 50 events,
which changed in none of 20 rounds on any node; only the machine's `sample`, `round_trip` and `taken_at`
change between rounds, 0.5 to 1 KB. Yet each relay pushed its whole snapshot every three seconds
under a new version, about 30% of the mesh was a neighbor resending a snapshot already on its way,
nothing was compressed, and `/live` sent every node's whole entry every three seconds, 5 GB a day
for one open page. Mesh sockets also jammed until the 90-second drop.

## Decision

Decided with the author on 2026-10-14, all of it, built in the order below, each step working
beside relays one step behind it during a rollout, negotiated in the stream's first message.

1. **A socket never jams on its own writes**: each mesh connection has a writer of its own, so a
   send waiting on a slow peer never stops the reading that would unblock it.
2. **Events leave the snapshot.** A page reads them from the runs mirror every relay already
   holds, `/runs?since=`, answered from memory as every relay answer is; `/live` and the mesh carry
   each node's runs version instead, a few bytes that move only at a deploy, and a page reads again
   when one moves.
3. **A new version only when content changes, and the readings kept where they are taken.** Apps,
   the machine's `info`, `stale` and `leaving` are the content. The machine's readings -- CPU,
   memory, load, network -- leave the snapshot, decided with the author on 2026-10-14:
   - **The node keeps its own, in the meter, rolled up as they age**: every one to three seconds
     for the last 15 minutes, in memory; a minute's mean and maximum for a day, and fifteen
     minutes' for twelve, on the meter's disk; hours kept in the meter's own file as today, never
     in the platform's Postgres, decided with the author on 2026-10-14. The meter is infra, beneath the platform, so it keeps them in
     a store of its own and never in the platform's databases or buckets, which run on top of it --
     RFCs 6jw04 and vh1bl are for apps above the line.
   - **The mesh carries a beat and a minute.** A **beat**, a few dozen bytes every three seconds,
     says only that a node is alive, so `live` within 10 seconds, `late` within 60 and the leaving
     announcement keep their meaning; once a minute each node sends its readings' mean and maximum,
     so every relay holds a coarse copy of every node.
   - **Fine readings are drawn while someone looks.** A page open on a node subscribes through its
     relay to that node's, a lease of 60 seconds renewed while the page stays; the origin streams its
     readings every few seconds to that relay alone, and stops when the lease lapses. **A history
     is read from the origin's host through its own binding, as the console already reads it**, and
     only an origin that does not answer is answered from the minutes the nearest relay holds; no
     request crosses the mesh for it, decided with the author on 2026-10-14 over asking the origin
     through the mesh, which reached the same meter by a new path.
   - **A threshold is judged on the node**, a reading high for long enough becoming content -- a
     new version, sent at once -- so an alarm needs nobody watching. Judged by the relay on the
     meter's minutes, the meter staying an instrument, with the values the author chose on
     2026-10-14: CPU mean at or above 90% for five minutes, memory used at or above 90% for five
     minutes, storage used at or above 90% at once, and any temperature zone at or above 85 C for
     five minutes.
4. **Full once, then only what changed, per connection.** Each connection keeps, per node, the
   version its other end holds; one that holds nothing is sent the node whole, and after that a
   delta from the version it holds, rows of apps diffed by name. **A delta names the version it
   applies to and carries the digest of its result**; a receiver holding another version, or reading
   another digest, asks for that node whole. **Every three seconds each side sends a sync** -- per
   node its version, an eight-byte digest and its latest beat -- which is the backstop: what differs
   is sent whole. No forward error correction: over a stream a message arrives whole or the stream
   drops, and redundancy dropped with it would buy nothing.
5. **The mesh is compressed**, a zstd stream per connection, so a node's next version costs about
   a hundred bytes against the last one sent.
6. **`/live` is Server-Sent Events**, not a WebSocket: it only ever sent one way, and as an event
   stream Caddy compresses it with no setting of its own. Its first event is the whole cluster, so
   the stream is never too short to compress.
7. **A relay reads its host by what is new**: events after the last id read, and apps by an ETag,
   not the newest page every round -- infra's half.
8. **The second phase: beats over UDP, everything else over the stream**, as HashiCorp's memberlist
   splits gossip from state sync, and as the author put it, a UDP heartbeat beside a TCP sync. A
   beat, 0.5 to 1 KB, fits one datagram inside the tailnet's MTU of 1,280 bytes, latest wins, and a
   lost one is superseded by the next; so a beat reaches a peer while its stream is stalled -- `rdu`
   held one at a congestion window of 1 and a 26-second retransmit -- which is what liveness needs.
   Deltas stay on the stream, since each needs its base applied first and the stream compresses
   them five to eight times; so do whole nodes and the sync. Each datagram carries a MAC and a
   sequence against replay, `12012/udp` is granted beside the stream's port, and a path through
   DERP gains nothing, since DERP carries datagrams over TCP. Built after step 7, and only if a node
   with a live socket is still seen `late` once step 1's writer is in.

Expected, per node a day: 19.6 GB today; about 2.4 after step 4; about 1.6 after step 6; about
0.5 after step 7. Across nine: about 134 GB a day to about 3.
