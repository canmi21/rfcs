---
name: Plain Markdown
title: An RFC is plain Markdown, CommonMark with GFM, and never calls a component of the system it decides
status: accepted
supersedes: [2ut6h]
blocked-by: []
commits: []
references:
  - url: https://spec.commonmark.org/
    took: the Markdown an RFC is written in
  - url: https://github.github.com/gfm/
    took: the extensions to it an RFC may use, tables among them
---

## Question

RFC 2ut6h had an RFC call a specimen, in the corpus's directive syntax, from components space
registers, so a comparison of two values could be shown rather than told. Those components are
drawn with the design system, and the RFCs are where the design system is decided. A frozen RFC
calling a component the system later drops, or draws differently, is either broken or shows what
was not decided. What may an RFC's body use?

## Options

| What an RFC's body may use                    | Against it                                                   |
| --------------------------------------------- | ------------------------------------------------------------ |
| Specimens called by directive, as now         | A frozen record that depends on what it decides, and changes |
| Raw HTML with its own inline styles           | The same dependence by another road, and unreadable as text  |
| Markdown alone, with its published extensions | A comparison is a table of values rather than a picture      |

## Decision

Markdown alone, with its published extensions.

- **An RFC's body is CommonMark with GFM**: tables, footnotes, strikethrough, task lists and
  autolinks, and nothing of ours. No directive, no HTML, no component.
- **A value is written as its value**: a color as its `oklch()`, a size as its length, in a table
  where there are several. The raw file says everything the page does.
- **Another extension is added only if it is published Markdown that draws plain elements**, the
  page styling them as it styles prose. One of ours, or one that needs a component, is never added.
- **The pages read RFCs with a parser of their own**, CommonMark and GFM alone, not the corpus's,
  so a directive written by habit reads as the text it is. `check-rfc` refuses HTML, and a line
  that opens as a directive would.
