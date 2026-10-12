---
name: 'Grays Step in Lab Lightness'
title: Every gray's step in the dark is 1.5 times the light's, measured in CIELAB L*, not in sRGB levels
status: landed
supersedes: []
blocked-by: []
commits: [lib@3fb4195a]
references: []
---

## Question

Every gray but the text keeps one relation between the themes: the dark's step off its base is
the light's times a ratio. The ratio had been 1.4, measured in sRGB levels, which is how a rank laid
thin blends. Measured that way the dark's surface came to 10 levels over black, and on anything
narrower than a card, a swatch's well or a band around it, it read as black and stood no higher than
the base. What scale is the step measured in, and at what ratio?

## Options

Each option keeps the light's grounds and the dark's ranks and borders as they were, and works the
other side out from them, each step on its own base.

| Measured in | Ratio | Dark surface, oklch L  | Light `subtle`, alpha | Light `border-strong`, alpha |
| ----------- | ----- | ---------------------- | --------------------- | ---------------------------- |
| sRGB levels | 1.4   | 0.145                  | 3.5 percent           | 17.1 percent                 |
| oklch L     | 1.4   | 0.029, black on screen | --                    | --                           |
| CIELAB L\*  | 1.4   | 0.156                  | 2.88 percent          | 21.0 percent                 |
| CIELAB L\*  | 1.5   | 0.159                  | 2.69 percent          | 19.7 percent                 |

## Evidence

oklch L is a cube root all the way to black, so a step the size of the light's, 0.021, is less than
one sRGB level once it is taken near black: the surface vanished into the base. sRGB levels are an
encoding, not a reading, and kept the surface at 10. CIELAB L\* runs straight near black, by what
the eye tells apart there, and put the surface between 0.14 and 0.18, where the author found it
read as gray when the values were switched in place on the design pages, 2026-10-11. Under L\* the
light's ranks also fall evenly, where under sRGB levels their ratios in L\* ran from 1.1 to 1.5.
1.5 read better than 1.4 beside it.

## Decision

1.5 in CIELAB L\*.

- **Every gray but the text steps off its base 1.5 times as far in the dark as in the light, in
  CIELAB L\***: the dark's surface off black, and each rank and border laid on the base.
- **The light's grounds and the dark's ranks and borders are set; the other side is worked out**:
  the dark's surface `oklch(0.159 0 0)`; the light's ranks 2.69, 3.14, 4.17 and 7.43 percent, its
  borders 4.17, 5.95 and 19.65 percent.
- **Each value is written out**, not derived in CSS, since CSS cannot take L\*; mono's test reads
  the stylesheet and holds every pair to the ratio.
- This replaces the 1.4 of RFC ic4cd, RFC h27v0 and RFC c656n, and nothing else they decided.
