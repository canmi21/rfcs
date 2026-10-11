---
name: Selectable Article Controls
title: "The article page's controls can be selected"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Outside the rail nothing on the article page is `select-none`, so its controls are text to a drag:
the code block's copy button and collapsible title row
(`libs/prose/src/blocks/code-block.svelte:361`, `:435`), the footnotes' fold toggle with its "Show N
more" (`apps/site/src/lib/article/footnotes.svelte:364`), the heading anchor
(`libs/prose/src/anchor-button.svelte:66`), the action bar holding reading progress and the theme
toggle (`libs/prose/src/action-bar.svelte:39`), and the video player's control bar, settings menu
and time readout (`libs/prose/src/components/video-chrome.svelte`, `video-controls.svelte:672`,
`video-settings.svelte:243-300`).

**What deciding it would cost.** A rule per control; the action bar and the player's bar are chrome
as well as controls, so `select-none` at their roots.
