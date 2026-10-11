---
name: A Commit Said Less
title: 'A commit said less than it carried, because the paths were a directory'
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`72f00943`, "fix: the twenty-six frame declarations leave the visual layer", also carries six
`:global()` selectors narrowed to their attribute form in `article.svelte` and `body.svelte`, and
the matching class writes removed from `compile.ts` and its test. Those belong to the anchor
migration's second step, not to the utilities move, and the message names only the second thing.

**The cause is the commit, not the workers.** It was taken as `jj commit apps/site/src ...` while
two workers were writing in that tree, and a path list takes everything under the path rather
than the change the message describes. The workspace's `spec/toolchain.md` already says a path
list silently omits what it does not name; this is the same mechanism running the other way, and
reading `jj st` in full before committing is what catches both.

Nothing is wrong in the tree -- both changes were verified and both are wanted. What is wrong is
that the log no longer separates them, so a bisect over the anchor migration lands on a commit
about utilities.
