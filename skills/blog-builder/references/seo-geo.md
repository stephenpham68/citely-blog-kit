# SEO / GEO — 2026 reality

Search is now an **answer layer**: winning means being *cited and trusted* by AI engines (AI Overviews, ChatGPT, Perplexity, Gemini), not just ranking #1. Sources in positions 4–20 get pulled into answers. Build for extraction.

## What's dead (don't build it)

- **FAQ rich results** — Google removed them entirely (May 2026); HowTo died 2023. Do **not** build FAQ-accordion features just to emit `FAQPage` schema. Q&A *content* still matters (below); the schema chase doesn't.
- **Fake `AggregateRating`.** Never emit rating schema from invented numbers — it's a spam signal and a trust violation.

## What wins (build this)

1. **Answer-first sections** — the #1 lever. Under every question-shaped H2, the first paragraph is a self-contained 40–90-word direct answer (self-contained passages get cited ~4×). Elaboration follows.
2. **TL;DR / quick-summary block** near the top — 3–5 bullet answers. The single strongest "citation magnet"; ranks above FAQ blocks in extraction studies.
3. **E-E-A-T signals** — real author with role + bio + archive page (`Person` schema), publisher identity (`Organization` schema + about box). Strongly correlated with AI-answer inclusion.
4. **Freshness** — emit `dateModified`, show "Last updated · {date}" when meaningfully edited. Updating old posts beats publishing new ones for AI pickup.
5. **Speed** — fast first paint materially increases citation odds. Lazy-load images, no blocking scripts, system-font fallbacks.
6. **Multimodal** — text + images (real `alt`) + tables + (optionally) video markup outperform text-only.

## Required JSON-LD (per article)

One `@graph` in a single `<script type="application/ld+json">`:

- `BlogPosting` — headline, description, image, `datePublished`, **`dateModified`**, author → `Person`, publisher → `Organization`, `mainEntityOfPage`.
- `BreadcrumbList` — Blog → Category → Article.
- `Person` (author) — name, jobTitle, url; `Organization` (publisher) — name, logo, url.
- Optional when genuinely present in content: `VideoObject`, `ImageObject`. `FAQPage` only if the article truly is Q&A-structured — cost ~0, but expect no rich result.

## Semantic HTML checklist

- One `<h1>`; H2/H3 strictly hierarchical (no skipped levels — the auto-TOC depends on it).
- `<article>`, `<nav aria-label="Table of contents">`, `<time datetime>`, `<figure>/<figcaption>`.
- Canonical URL, Open Graph + Twitter card meta, descriptive `<title>` ≤ 60 chars, meta description ≤ 160.
- Internal links: descriptive anchor text, brand-colored + bold in prose (they're navigation *and* topical-authority signals).
