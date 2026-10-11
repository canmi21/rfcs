---
name: A Wrap Policy per Language
title: 'A wrap policy is one decision per language, and the paragraph is where it is wanted'
status: draft
supersedes: []
blocked-by: [lqpnt]
commits: []
references: []
---

## Question

web's `spec/styling/prose.md` settles where a line ends per language and per column width,
and both halves hold. What it has no way to say is that one paragraph wants a different answer from
the one beside it, and that is the case English keeps producing: a two-line paragraph whose second
line carries three words. Measured on `convention/forecast-tense` at the 672px column, five of its
twelve multi-line paragraphs end on a line holding between 16% and 32% of the column; the whole
article is nineteen paragraphs and thirty-four lines, so the shape is most of what a reader sees.

`text-wrap: pretty` does not reach this. Measured over that article it scores identically to the
browser's default -- same lines, same stranded finals -- because rescuing a three-word final line
means loosening the line above it, and that is the trade `pretty` declines. `balance` is the
property that makes the earlier lines shorter, and applied to the whole article it costs what
prose.md already says it costs: the mean gap on non-final lines goes 3.1% to 24.3% and eleven of
twenty-two lines stand more than an eighth short.

**Applied per paragraph instead, the same property costs about half.** Marking only the paragraphs
whose final line falls under a threshold, measured on `architecture/compile-time-rendering` --
forty-nine paragraphs, 224 lines, no extra lines at any setting:

| threshold      | marked | short finals | mean gap | loose lines |
| -------------- | ------ | ------------ | -------- | ----------- |
| none, as it is | 0      | 8            | 2.0%     | 0           |
| 15%            | 3      | 5            | 3.2%     | 10          |
| 20%            | 4      | 4            | 3.6%     | 13          |
| 30%            | 6      | 2            | 4.2%     | 21          |
| 40%            | 8      | 2            | 4.7%     | 26          |
| whole article  | 49     | 2            | 7.9%     | 40          |

Two paragraphs survive every threshold because Chromium stops balancing past six lines; the two
are eleven lines and eight. A system that marks paragraphs should not mark those -- the declaration
would be inert and the marking would lie about what the page does.

**The rule cannot be written in CSS, and that is the whole shape of the problem.** A selector
cannot ask how long a line came out, so something that can see the layout has to decide, and only
then does CSS execute the answer under a policy name. Three places could decide. The browser, with
the `measured()` mechanism web's `spec/styling/first-paint.md` already uses for the
table of contents rail -- exact at any width, at the price of a first frame that reflows and of
JavaScript in the reading path. The compile step, laying the text out with the real font at the one
column width that is fixed -- exact only if its line breaking agrees with the browser's, and unable
to say anything about the narrow column, whose width is the device's. Or a character count, which
was measured and is not viable: predicting from characters per line picked two of four paragraphs
on the short article and three of eight on the long one, with seventeen false positives.

That the fixed column and the per-paragraph decision share a boundary is not a coincidence. A
paragraph can only be judged where the column is a known number, which is `--rail-column` and
above -- exactly where `pretty` already lives.

What deciding it costs: a named policy per language, which is the shape prose.md already has; an
optional per-paragraph override in the compiled article, which is a change to the block contract in
`@monoflake/sdk/artifacts` and to both ends that read it; and a measuring layer chosen from the three above.
The threshold is a constant in whichever layer measures, and the table above is what it should be
argued from. The language half stays as it is: English opts in, every other language keeps one
answer per article until somebody measures it.
