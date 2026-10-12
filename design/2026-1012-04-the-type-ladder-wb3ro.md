---
name: 'The Type Ladder'
title: The sizes text is set in, named for what each sets, from a caption to a display
status: landed
supersedes: []
blocked-by: []
commits: [lib@8f5e7c27, web@38b1d28e]
references: []
---

## Question

The scale names three sizes, `caption`, `label` and `body`, and stops there. Above them, the text
space and the console draw is set at sizes written by hand and marked unnamed. Which steps does the
ladder hold above `body`, what is each named for, and where two stand this close, are they one?

## Options

- **Name each size in use as it stands**: `prose` 1rem for text to be read, `title` 1.25rem for a
  section, `headline` 1.5rem for a page, `figure` 1.875rem for a number read at a glance, and
  `display` 2rem for a document, the largest.
- **The same, with `figure` folded into `display`**: the console's stat tile grows 2px and the
  ladder keeps one step fewer.
- **A ratio from `body`**, each step a fixed multiple of the last, the sizes in use moved onto it.

## Evidence

Sizes and tracks written by hand across space's design and RFC apps, its shared library and the
console:

| Size     | Where                                                                  |
| -------- | ---------------------------------------------------------------------- |
| 1rem     | a card's sentence, the prose, the corner's name                        |
| 1.25rem  | a section's title in the prose and in an RFC, a figure in the overview |
| 1.5rem   | a card's title, the RFC list's title, the console's page title         |
| 1.875rem | the console's stat tile                                                |
| 2rem     | an RFC's title                                                         |
| -0.02em  | the tracking under a card's title and the console's page title         |
| -0.03em  | the tracking under the stat tile                                       |

The design pages are drawn with the system they show, so their own titles are set at a size the
system does not name.

The console's stat tile at 1.875rem and an RFC's title at 2rem stood 2px apart, two steps no reader
told from one; set at 2rem, the tile read as it had.

Tailwind already holds the t-shirt sizes for type, at other values -- its `text-sm` is 14px -- so the
same word would mean two sizes in files that mix the two; a number would name only an order, which a
reader takes for a share of the body.

## Decision

Name each size in use, with `figure` folded into `display`: seven steps, named for what each sets.

- **The interface**: `caption` 0.75rem, `label` 0.8125rem, `body` 0.875rem.
- **Text to be read**: `prose` 1rem, the browser's own size.
- **Headings**: `title` 1.25rem for a section, `headline` 1.5rem for a page, `display` 2rem for a
  document; **a figure read at a glance is set at `display`**.
- **Never a t-shirt size or a number** for a type size.
