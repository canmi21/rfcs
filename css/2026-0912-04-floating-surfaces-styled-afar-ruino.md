---
name: Floating Surfaces Styled Afar
title: The floating surfaces are styled from the component that summons them
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`.menu-content`, `.popover-content`, `.modal-overlay`, `.modal-content`, `.search-panel`,
`.search-overlay`, `.preview-stage`, `.preview-ground` are all reached with `:global` because Bits
UI portals them out of the component tree. Same shape as the paragraph above and a different cause:
here the element exists and is ours, it is simply somewhere else in the document.
