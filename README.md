# my-project

## Typography

An editorial type system for the landing page: **Instrument Serif** for display,
**Inter** for body and UI.

```
index.html              demo landing page + live specimen of the scale
styles/tokens.css       families, size scale, leading, tracking, measure, ink
styles/typography.css   element defaults (h1–h6, p, lists, quotes, code) + .t-* utilities
styles/page.css         layout chrome for the demo page only — delete when porting
styles/motion.css       hero entrance + button feedback; the only file that animates
```

Open `index.html` directly, or serve it:

```sh
npx http-server . -p 8080
```

### How it's built

- **Fluid scale.** Eleven sizes, `clamp()`ed between a 360px and ~1400px
  viewport, all in `rem` so the reader's browser text-size setting still works.
- **Tracking follows size.** `-0.016em` on the hero, `0` on body copy,
  `0.12em` on uppercase labels. No single letter-spacing value for everything.
- **Leading runs inverse to size.** `1.0` on the hero, `1.65` on body,
  `1.75` in `.t-prose`.
- **Measure is a token.** Paragraphs cap at `66ch`, leads at `46ch`.
- **Colour is one family.** Every ink sits in the same warm hue as the paper
  (red channel above blue), because cool greys on a warm surface read dead.
  The accent carries the uppercase labels, list markers, quote rules and link
  underlines — not just focus rings.
- Light and dark, via `prefers-color-scheme` and overridable with
  `data-theme="light" | "dark"` on `<html>`. Both themes are tuned to the same
  contrast ratios, so the accent has equal weight in each. All text clears
  WCAG AA.

### Using it

Every type value lives in `tokens.css` — nothing else in the project should
contain a `font-size`. A section opens with `.t-eyebrow`, then an `h2`, then a
`.t-lead` if it needs a sentence of orientation. Body copy needs no class.

| Utility | For |
| --- | --- |
| `.t-hero` | the one headline above the fold |
| `.t-lead` | sub-headline / intro paragraph |
| `.t-prose` | long-form reading blocks |
| `.t-caption` | captions, metadata, footnotes |
| `.t-eyebrow` | uppercase label above a headline |
| `.t-pull` | pull quote inside a long section |
| `.t-numeric` | tabular figures for prices and tables |
| `.t-measure-*` | override line length where `66ch` is wrong |

### Motion

`styles/motion.css` is the only file that animates anything, so the type system
stays free of motion concerns.

- **Hero entrance.** The four hero children fade and rise 10px in sequence
  (0 / 60 / 120 / 180ms), 500ms on `cubic-bezier(0.23, 1, 0.32, 1)`. A CSS
  animation rather than JS so it runs off the main thread — the page is
  fetching two webfonts at exactly that moment.
- **Buttons.** 1px lift on hover, gated behind
  `@media (hover: hover) and (pointer: fine)` so a tap doesn't leave a stuck
  hover state; `scale(0.97)` on press, which takes the label with it.
- **Reduced motion.** Gentler, not gone: the hero still fades so the page
  doesn't snap into place, buttons keep colour feedback, all movement drops.

### Swapping the faces

Change `--font-display` / `--font-body` in `tokens.css` and update the Google
Fonts `<link>` in the `<head>`. Instrument Serif ships a single weight, which is
why the heading rules set no `font-weight` and why `font-synthesis-weight: none`
stops the browser faking a bold. A multi-weight display face wants an explicit
`font-weight` added to the heading rule.

Fonts load from Google Fonts with `preconnect` + `display=swap`. To drop the
third party, self-host the `.woff2` files and replace the `<link>` with local
`@font-face` rules.
