# Blog hub — overview page standard

The hub is a **topic-organized magazine front**, not a flat reverse-chronological list.

## Page anatomy (top → bottom)

1. **Header** — white, site nav + category nav. Each category link gets a **color dot** + post-count badge. Category colors come from a fixed map (e.g. amber/indigo/violet/emerald/cyan/rose) used consistently for dots and cover fallbacks — but **meta text stays neutral gray** (`--muted-foreground`), never category-colored.
2. **Hero band** — compact (~280px tall, don't let heroes bloat), `--brand-rich` gradient, white text: eyebrow pill (dynamic year if it mentions one), one-line H1, one-line subtitle, dual CTA (primary white / secondary outline), optional 2–3 stat chips.
3. **Spotlight row** — two columns:
   - **Featured card** (left, ~2/3): newest or hand-picked post — large cover, category badge, title, excerpt, byline.
   - **Most-read rail** (right, ~1/3): "🔥 Most read" top 5–7 list, gradient-numbered (1–7), title + meta each. **Fixed count, no scrolling inside the rail.** Stretch to match featured-card height (`flex: 1` + `justify-content: space-between`), "View all →" pinned at the bottom. Source from real analytics (e.g. Search Console clicks, 7 days) — if no data yet, fall back to newest and say so; never fake "most read".
4. **Topic sections** — one section per category: color dot + section name + one-line description + meta ("N posts · N min read") + grid of **6 compact cards** + "View all N →" + **one product CTA relevant to that topic**. Compact card = left thumbnail (cover or category-gradient fallback), 2-line clamped title, neutral-gray meta.
5. **News/updates section** (optional) — distinct label treatment (e.g. "● BRIEFING") and a tinted card style if the blog runs a news digest category.
6. **Closing conversion band** — full-width gradient: "Done reading — let {product} do it for you?" + up to 4 CTAs.
7. **Footer** — white, dynamic year.

## Surface

- Hub body background = `--blog-surface` (subtle brand tint, **light mode only**); header + footer stay white; dark mode keeps the neutral dark background untinted.
- Cards are white on the tinted surface — the tint is what makes the hub feel branded without shouting.

## Rules

- Titles clamp to 2 lines (`-webkit-line-clamp: 2`); excerpts to 2–3 lines. No layout jumps from long titles.
- Every post card is one `<a>` block (whole card clickable), with visible focus style.
- Section CTAs and the closing band promote the site's own product only.
- Pagination or "View all" per category — don't infinite-scroll the entire hub.
- Optional power feature: ⌘K/Ctrl-K search palette (diacritics-insensitive matching for Vietnamese and similar languages).
