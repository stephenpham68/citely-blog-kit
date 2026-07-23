# Article page — reading UX standard

The article page is the product. Every element below has a purpose; build them in this order.

## Page anatomy (top → bottom)

1. **Header** — site nav, white background, one primary CTA. Sticky optional.
2. **Reading progress bar** — 3px fixed at viewport top, fills with scroll depth, brand gradient.
3. **Breadcrumb** — `Blog / {Category}` (also emitted as `BreadcrumbList` JSON-LD).
4. **Article head** (centered column, max ~800px):
   - Category badge (brand-soft background) **+ publish date** on the same row.
   - H1: `clamp(32px, 5.6vw, 54px)`, weight 900, line-height ~1.08, near-black (#3a3a3a-style, not pure black).
   - Compact byline: avatar + author name + short role. (Full bio lives in the author box at the foot — don't duplicate.)
   - Keep the head lean: no excerpt/dek repetition, no fake view counts.
5. **Cover image** — 16:9, rounded (`--radius-figure` × 1.3), inside the content column. Fallback when no image: brand gradient + oversized watermark word.
6. **Body layout** — 3-column desktop grid: `60px (share rail) | minmax(0,1fr) (prose) | 268px (TOC)`, gap ~38px, container max ~1140px.
7. **Article foot** — rating input → about-the-site box → author box → updated-date note.
8. **Related posts** — card slider/grid.
9. **Closing conversion band** — full-width brand gradient, headline + 2–4 CTA buttons (own product only).
10. **Footer** — white, dynamic year.

## Table of contents (the signature feature)

- **Always auto-generated** at render time: parse the article's H2/H3, assign slug ids, build a nested outline. Never hand-authored; strip any manual "Mục lục"/"Table of contents" list found inside content.
- **Two levels:** H2 items numbered (1, 2, 3…); H3 items indented with a small dot marker.
- **Desktop:** sticky right rail (`position: sticky; top: ~88px`), scrollable if long.
- **Scroll-spy:** IntersectionObserver highlights the current H2 *and* H3 (brand color + bold). Give headings `scroll-margin-top: 88px` so anchors don't hide under the sticky header.
- **Anchor clicks:** smooth-scroll via `scrollIntoView` with `preventDefault` so the URL stays clean (no `#hash`).
- **Mobile (≤1000px):** rails disappear; render a collapsed `<details>` block ("📑 In this article") above the content, built from the same outline, same scroll-spy.

## Share rail

- Desktop: vertical sticky rail left of prose — Facebook, X, LinkedIn, copy-link (with "copied" feedback).
- Mobile: the same buttons become a horizontal row in the content flow (readers must still be able to share).

## Prose — style bare tags

Article HTML arrives with **no classes** (see `content-contract.md`), so all styling targets bare tags inside `.prose`:

- Column: max-width ~740px; paragraphs capped at ~70ch; font-size 17–18px; line-height ~1.7; color `--body`.
- `h2`: display font, weight 800, **brand color** (light mode; dark mode keeps foreground), `1.9em` top margin.
- `h3`: weight 700, `--foreground`.
- `strong`: `--foreground` (pops against the softer `--body` gray). `a`: brand color + weight 600, underline on hover.
- `table`: rounded 14px outer frame (`border-collapse: separate` + wrapper), header row bold on muted tint, `caption` rendered as a brand-gradient banner (caption-side: top, uppercase). **Wrap tables in an `overflow-x: auto` container** — wide tables scroll, the page never does.
- `img`/figures: 16:9 where possible, `--radius-figure`, `loading="lazy"`, always with `alt`.
- `blockquote`: left brand border + soft tint background.
- `ul/ol`: comfortable spacing; support a ✓-checklist style for lists of criteria.
- Code blocks (if the blog is technical): monospace block with copy button.

## Content blocks

- **TL;DR callout** ("Tóm tắt nhanh" / "Quick summary") near the top: bordered card, 3–5 bullet answers. This is the #1 block AI engines cite — see `seo-geo.md`.
- **Rating widget** — input-only: "Was this helpful?" + 5 stars with hover labels + thank-you state. **No fabricated average, no fake count, no AggregateRating schema** until real persisted data exists.
- **About-the-site box** — 2–3 lines about the publisher + 2–3 internal product links, left brand border (E-E-A-T entity clarity).
- **Author box** — avatar, name, role, short bio, link to author archive (E-E-A-T).
- **Updated note** — "Last updated · {date}" at the foot, only when meaningfully edited after publish; feeds `dateModified`.
- **Related posts** — horizontal snap slider (`overflow-x: auto` + `scroll-snap-type: x mandatory`), cards ~85% width on mobile with peek, 3-up grid on desktop; ‹ › buttons only when overflowing.
- **Right-rail CTA card** — one product CTA card below the TOC (gradient card). Never third-party ads.

## Motion (all gated by `prefers-reduced-motion`)

- Reading progress bar + circular progress ring around the back-to-top button.
- Scroll-reveal fade-up on major blocks — **default-visible**: content must render if JS fails; JS hides then reveals, never the reverse.
- Subtle hover: cover image zoom (~1.05), card lift, CTA lift.
- Skip: parallax, confetti, cursor effects — unless the user explicitly asks.
