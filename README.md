# my-project

## Typography

An editorial type system for the landing page: **Instrument Serif** for display,
**Inter** for body and UI.

```
index.html              demo landing page + live specimen of the scale
styles/tokens.css       families, size scale, leading, tracking, measure, ink
styles/typography.css   element defaults (h1–h6, p, lists, quotes, code) + .t-* utilities
styles/page.css         layout chrome for the demo page only — delete when porting
```

Open `index.html` directly, or serve it:

```sh
npx http-server . -p 8080
```

### How it's built

- **Fluid scale.** Eleven sizes, `clamp()`ed between a 360px and ~1400px
  viewport, all in `rem` so the reader's browser text-size setting still works.
- **Tracking follows size.** `-0.032em` on the hero, `0` on body copy,
  `0.12em` on uppercase labels. No single letter-spacing value for everything.
- **Leading runs inverse to size.** `1.0` on the hero, `1.65` on body,
  `1.75` in `.t-prose`.
- **Measure is a token.** Paragraphs cap at `66ch`, leads at `46ch`.
- Light and dark, via `prefers-color-scheme` and overridable with
  `data-theme="light" | "dark"` on `<html>`.

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

### Swapping the faces

Change `--font-display` / `--font-body` in `tokens.css` and update the Google
Fonts `<link>` in the `<head>`. Instrument Serif ships a single weight, which is
why the heading rules set no `font-weight` and why `font-synthesis-weight: none`
stops the browser faking a bold. A multi-weight display face wants an explicit
`font-weight` added to the heading rule.

Fonts load from Google Fonts with `preconnect` + `display=swap`. To drop the
third party, self-host the `.woff2` files and replace the `<link>` with local
`@font-face` rules.
