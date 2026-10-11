---
name: English Told It Has No English
title: An article written in English is told it has no English version
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Open `/hindsight/except-me` or `/convention/forecast-tense` in the English interface and the
translation notice reads "No English version of this article yet. What is shown is English (US)".
Both carry `lang: en`, so the sentence contradicts itself in the one place a reader sees.

It is not in the server's markup -- `grep translation-notice` over either response returns
nothing -- so it arrives at hydration, from `article.svelte`'s
`{#if locale.code !== 'mw'}` around the notice. That test asks whether the reader is looking at a
non-default locale; it never asks whether the article's own `lang` already is that locale. For a
corpus written mostly in Chinese the two questions had the same answer, and the first article in
English made them differ.

The fix is a condition, not a message: the notice belongs where the view's locale differs from
`meta.lang`, which the component already receives as `sourceLanguage`. Worth checking at the same
time what the notice should say on a `lang: en` article viewed in Chinese, since that is the
mirror case and nothing has exercised it either.
