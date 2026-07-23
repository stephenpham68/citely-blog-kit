---
name: blog-builder
description: Build or upgrade a professional blog UI (blog hub + article reading experience) for any website or platform, following the Citely blog standard. Use when creating a blog from scratch, styling article HTML received from a CMS or Citely Connect, adding a table of contents / reading progress / typography system to articles, or auditing an existing blog against SEO/GEO and reading-UX standards.
---

# Blog Builder — Citely blog standard

Build two surfaces to a defined quality bar: the **blog hub** (overview/index page) and the **article page** (reading UX). Everything is token-first and brand-agnostic: the standard fixes structure, hierarchy, and quality gates; the user's brand supplies color and type.

## When to use

- The site has no blog, or has a bare "list of posts" that needs a real hub + reading UX.
- Article HTML arrives from a CMS/API (e.g. Citely Connect) as bare tags and must be rendered beautifully.
- The user asks for a TOC, reading progress, better typography, tables, dark mode, or mobile fixes on a blog.
- Auditing an existing blog page against this standard (use `references/checklist.md`).

## When NOT to use

- Marketing landing pages, docs sites, or e-commerce category pages — different information architecture.
- Full CMS/backend design (storage, editor, publishing pipeline) — this skill covers the *reading* surfaces only.

## Required inputs (ask if missing)

1. **Brand color** (one hex). Derive the rest per `references/design-tokens.md`. Never invent a brand palette without asking.
2. **Font(s)** — a display/heading font and a body font (can be the same family).
3. **Site language** for all UI strings (labels, CTA, dates). Article content keeps its own language.
4. **Stack** — plain HTML/CSS, React/Next, Astro, Vue, PHP theme… The standard is stack-agnostic; templates are vanilla HTML/CSS/JS you translate to the stack.
5. **Content source** — where articles come from (API shape, fields). If Citely Connect, see `references/content-contract.md`.

## Workflow

Work in this order — each step has a reference file with the concrete spec:

1. **Tokens first** — set up CSS custom properties (brand, neutrals, surface tint, type scale, radii) per `references/design-tokens.md`. All later CSS consumes tokens; no hardcoded colors in components.
2. **Article page** — the highest-value surface; build it before the hub. Follow `references/article-page.md`: layout grid, auto-generated 2-level TOC with scroll-spy, reading progress, prose styles for bare tags (p, h2, h3, table, img, blockquote, ul/ol), TL;DR callout, author + about boxes, related posts, one closing CTA band.
3. **Blog hub** — follow `references/blog-hub.md`: compact hero band, spotlight (featured + most-read rail), topic sections with compact cards, closing conversion band.
4. **Responsive pass** — apply the explicit tiers in `references/responsive.md`. Verify at 390px and 820px: zero horizontal overflow, tap targets ≥ 44px.
5. **SEO/GEO pass** — JSON-LD (`BlogPosting`, `BreadcrumbList`, `Organization`, author `Person`), semantic HTML, `dateModified`, answer-first guidance per `references/seo-geo.md`.
6. **Definition of Done** — run `references/checklist.md` before reporting the blog finished.

Start from `templates/article.html` and `templates/hub.html` when the stack allows: they are working single-file reference implementations of steps 1–5. Translate structure and behavior into the user's stack rather than pasting blindly.

## Hard rules (failure patterns to avoid)

- **No fake numbers.** Never render invented ratings, view counts, or review totals. A rating widget may collect input, but shows no fabricated average and emits no `AggregateRating` schema until real data exists.
- **No third-party ads.** CTA slots (right rail, closing band) promote the site's own product/service only.
- **No hardcoded TOC.** The TOC is always generated from the article's H2/H3 at render time. If content contains a manually-authored inline TOC, strip or ignore it.
- **No hardcoded year** in chrome (footer ©, hero pills) — render the current year dynamically. Exception: dates inside article content stay as authored.
- **No brand color for body text.** Long-form text uses neutral foreground tokens; brand color is reserved for H2, links, and accents — and must pass WCAG AA where used on text.
- **Motion is gated.** Every animation respects `prefers-reduced-motion`; content must never be invisible without JavaScript.
- **Contrast is checked, not assumed.** Small brand-colored text on light backgrounds usually fails AA — use the darkened `--brand-strong` variant.

## Output format

Ship real files in the user's stack, then report: what was built, which reference specs were applied, the checklist result (pass/fail per line), and what was deliberately deferred (e.g. most-read rail without analytics data).

## References

| File | Contents |
|---|---|
| `references/design-tokens.md` | Token set, brand-color math, dark mode, contrast rules |
| `references/article-page.md` | Article layout, TOC, prose, tables, E-E-A-T blocks, motion |
| `references/blog-hub.md` | Hub hero, spotlight, topic sections, conversion band |
| `references/responsive.md` | Breakpoint tiers, mobile TOC/share behavior |
| `references/seo-geo.md` | Schema JSON-LD, answer-first/TL;DR, freshness (2026 reality) |
| `references/content-contract.md` | Rendering bare-tag article HTML; Citely Connect field mapping |
| `references/checklist.md` | Definition of Done |
