# Definition of Done — run before calling the blog "finished"

Report pass/fail per line. Anything failed is either fixed or explicitly deferred with a reason.

## Tokens & theme
- [ ] All colors flow from `:root` tokens; zero raw hex in component CSS (except token definitions).
- [ ] Brand text on light backgrounds passes WCAG AA 4.5:1 (used `--brand-strong` where needed).
- [ ] Dark mode works on both surfaces (no light-only tints leaking, images/tables legible).

## Article page
- [ ] TOC auto-generates from H2/H3 (2 levels, numbered H2 / dotted H3), scroll-spy highlights current section, anchors don't hide under sticky header, URL stays clean on click.
- [ ] Reading progress bar tracks scroll; back-to-top with progress ring.
- [ ] Prose: bare-tag styling covers p, h2, h3, a, strong, ul/ol, blockquote, img, table (rounded frame, caption banner, header tint, `overflow-x` wrapper).
- [ ] TL;DR/quick-summary block present; share works (3 networks + copy-link with feedback).
- [ ] Author box + about-the-site box + updated note render (and hide gracefully when data missing).
- [ ] Rating widget (if included) shows **no fabricated numbers** and emits **no AggregateRating**.
- [ ] Related posts render; slider snaps and swipes on mobile.
- [ ] Closing CTA band promotes own product only; no third-party ads anywhere.

## Hub
- [ ] Hero ≤ ~300px tall; dynamic year in chrome strings; category nav has dots + counts.
- [ ] Spotlight: featured + most-read rail (fixed count, no inner scroll, real data or labeled fallback).
- [ ] Topic sections: 6 compact cards, 2-line title clamp, neutral-gray meta, per-section CTA.

## Responsive
- [ ] 390px and 820px: zero horizontal overflow (`body.scrollWidth ≤ innerWidth`).
- [ ] ≤1000px article: rails hidden, share row horizontal, mobile `<details>` TOC works.
- [ ] Tap targets ≥ 44px; tables scroll inside their wrapper.

## SEO/GEO
- [ ] JSON-LD `@graph`: BlogPosting (+`dateModified`), BreadcrumbList, Person, Organization — validates in Rich Results / Schema validator.
- [ ] One H1; heading hierarchy strict; `<time datetime>`; canonical + OG/Twitter meta.
- [ ] Images lazy-loaded with real `alt`; no render-blocking scripts.

## Robustness
- [ ] With JavaScript disabled: all content visible (reveal animations default-visible), TOC `<details>` still opens.
- [ ] `prefers-reduced-motion`: animations off, nothing broken.
- [ ] Console: zero errors on both pages.
