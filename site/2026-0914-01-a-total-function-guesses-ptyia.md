---
name: A Total Function Guesses
title: 'A total function answers for input it does not know, and is wrong instead of failing'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

web's `apps/local/src/extension.rs` maps a mime type to the extension a
stored file is named by. It returns `&'static str` rather than an option, and its own doc says
why: "Total rather than optional: the encoder only ever produces these, and anything unrecognised
is AVIF because that is what the ladder stores." That was true of a repository storing one kind of
asset. It stopped being true when video landed, and the function did not change, because nothing
about its signature could tell it had.

**What it does with a mime it does not know is answer `avif`.** So `for_variant("video/mp4")` is
`"avif"`, and a caller that hands it a clip's variant gets back a path under `image/` with an
extension nothing ever wrote. The failure has no error to surface and no branch to test: the
function cannot fail, so the caller cannot check, so the wrong path flows to `is_file()`, which
answers false, which reads as "this asset is not published yet".

It has already produced one bug of exactly that shape. `published()` in
web's `apps/local/src/image/run.rs` was written as
`variant_path(public, cid, extension::for_variant(&record.mime)).is_file()`, which is correct for
every picture. Copied into the video command unchanged -- the natural thing to do, because it
reads as a general question about a record -- it asks whether `{ab}/{cd}/{cid}.avif` exists for a
rung that lives at `{ab}/{cd}/{cid}.mp4`, one tree and one id apart only by extension. It answers
false on every run, and the
command re-encodes a clip that is already on disk. Nothing raises, nothing logs, and the only
symptom is that a run which should be a no-op takes minutes.

The shape is what invites this rather than any one call site. A function that takes a mime and
cannot fail reads as a function that knows every mime, so it gets called on mimes it does not
know, and the answer it gives is indistinguishable from an answer it does know. The separate
`for_icon` beside it is `Option`-returning for a reason its own doc gives -- a `Content-Type` from
a server nobody controls -- and the two sit in one file disagreeing about whether an unknown type
is a value or a question.

**What deciding it would cost.** The narrow reading is that `for_variant` is named for an image
variant and video should never have reached it, so the fix is at the call sites and the function
is fine. The wider reading is that a total function over an open input set is the defect, and
`for_variant` should return `Option<&'static str>` with the AVIF default moved to the one caller
that wants it -- which is every image path, so the change is small in edits and large in what it
asserts. Both readings leave a picture, a rung and a track needing a mime-to-extension answer
each, and there is no longer a tree in the key to tell them apart -- the extension is the whole of
it. Neither is worth doing while the video commands are still landing, because the call sites are
what would move.
