---
name: Selectable Page Chrome
title: "The licenses and error pages' chrome can be selected"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

The licenses pages' breadcrumb navs (`apps/site/src/routes/licenses/+page.svelte:137`,
`licenses/[license]/+page.svelte:88`, `licenses/pkgs/+page.svelte:99`,
`licenses/pkgs/[registry]/+page.svelte:85`,
`licenses/pkgs/[registry]/[...package]/+page.svelte:200`) and their action rows (`:193`, `:109`,
`:103`, `:235`) carry no `select-none`, where the article's equivalent rail does; the error page's
report button drawn as a link inside a sentence (`apps/site/src/lib/error/offer.svelte:77`) is
selectable too.

**What deciding it would cost.** `select-none` on those navs and rows; the inline button is a
control inside prose, the case most worth deciding explicitly.
