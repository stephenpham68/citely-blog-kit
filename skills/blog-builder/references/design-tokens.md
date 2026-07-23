# Design tokens — token-first theming

Define everything as CSS custom properties on `:root` before writing any component CSS. Components may only consume tokens.

## Token set (minimum)

```css
:root {
  /* Brand — user supplies --brand; derive the rest */
  --brand: #0e7490;               /* the ONE input from the user */
  --brand-strong: #0c637c;        /* darkened ~10–15% — for small text on light bg (must pass AA 4.5:1) */
  --brand-rich: linear-gradient(135deg, /* 3–4 stops around --brand's hue, warm→deep */);
  --brand-soft: color-mix(in srgb, var(--brand) 10%, white); /* tint washes, badges */

  /* Neutrals */
  --background: #ffffff;
  --foreground: #1c1c1f;          /* headings, strong */
  --body: #5f5f5f;                /* long-form paragraph text — softer than headings */
  --muted-foreground: #9a9aa6;    /* meta lines: date · read time (neutral gray, NOT category color) */
  --border: #e7e7ec;

  /* Surface tint — hub background only, light mode only */
  --blog-surface: color-mix(in srgb, var(--brand) 6%, white); /* e.g. orange brand → warm peach #fff4ee */

  /* Type */
  --font-display: /* heading font */, system-ui, sans-serif;
  --font-sans: /* body font */, system-ui, sans-serif;

  /* Shape */
  --radius-card: 16px;
  --radius-figure: 14px;
}
```

## Rules

1. **One brand input.** Ask the user for a single brand hex; compute `--brand-strong` (darker, AA-passing for text), `--brand-soft` (10% tint), and a `--brand-rich` gradient (3–4 stops moving through neighboring hues, e.g. Citely's `#f7931e → #f15a24 → #ed1c24 → #c81d6b`). Use the gradient only on hero bands, cover fallbacks, and the closing CTA band — never on text.
2. **Contrast is verified, not eyeballed.** Any brand-colored text < 24px on a light background must pass WCAG AA 4.5:1 — usually that means `--brand-strong`, not `--brand`. Body text `--body` on `--background` must also pass 4.5:1.
3. **Three text grays, not one.** Headings/strong use `--foreground`; paragraphs use `--body`; meta lines use `--muted-foreground`. This is what makes bold text, links, and H2s "pop" in long articles.
4. **Surface tint discipline.** The hub page body may take `--blog-surface` (a barely-there brand tint) in **light mode only**; header and footer stay `--background` (white). The article page content column stays white for reading comfort. Dark mode never tints — it keeps the neutral dark background.
5. **Dark mode.** Every token gets a dark value (`.dark` class or `prefers-color-scheme`). Brand gradients dim slightly; surface tint is dropped; figure/table borders lighten. Never ship light-only.
6. **Typography scale.** Body prose 17–18px, line-height 1.6–1.7. Headings use `clamp()`: H1 `clamp(32px, 5.6vw, 54px)` weight 800–900, tight letter-spacing (−0.02em); H2 = 1.5× prose size; H3 = 1.18× prose. Display font on headings, body font on prose.
7. **No raw hex in components.** If a component needs a color that has no token, add the token first.
