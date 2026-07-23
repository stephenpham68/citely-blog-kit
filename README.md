# blog-claudekit

**A Claude Skill kit that teaches any LLM to build a professional blog UI for your website — following the Citely blog standard.**

Citely ([citely-seo.com](https://citely-seo.com)) delivers SEO/GEO-optimized articles to any platform via Citely Connect. But many platforms (custom sites, Cloudflare Pages, static sites, headless CMS frontends…) don't ship with a proper blog UI. This kit closes that gap: drop it into your repo, and your coding agent (Claude Code, Cursor, Codex…) knows exactly how to build a blog hub + article reading experience that meets modern SEO/GEO and reading-UX standards.

> 🇻🇳 **Tiếng Việt:** Bộ skill giúp LLM của bạn tự build giao diện blog chuẩn (trang tổng quan + trang bài viết) cho website của bạn — mục lục tự sinh, thanh tiến độ đọc, bảng đẹp, responsive, schema SEO/GEO 2026 — theo tiêu chuẩn Citely gợi ý. Cài vào repo, ra lệnh cho agent là xong.

## What's inside

```
skills/blog-builder/
├── SKILL.md                  # Agent instructions (Agent Skills standard)
├── references/
│   ├── design-tokens.md      # Token-first theming: brand color, type scale, dark mode, contrast
│   ├── article-page.md       # Article reading-UX: layout grid, auto-TOC, prose, tables, E-E-A-T blocks
│   ├── blog-hub.md           # Blog overview: hero, spotlight + most-read rail, topic sections
│   ├── responsive.md         # Breakpoint tiers + mobile rules (tap targets, no horizontal overflow)
│   ├── seo-geo.md            # 2026 SEO/GEO: schema JSON-LD, answer-first, TL;DR, freshness
│   ├── content-contract.md   # How article HTML arrives (e.g. from Citely Connect) & how to render it
│   └── checklist.md          # Definition of Done before calling the blog "finished"
└── templates/
    ├── article.html          # Single-file reference implementation (article page)
    └── hub.html              # Single-file reference implementation (blog hub)
```

## Install

**Claude Code** — copy the skill into your project (or your user folder):

```bash
git clone https://github.com/<you>/blog-claudekit
cp -r blog-claudekit/skills/blog-builder your-project/.claude/skills/blog-builder
```

Then in your project:

```
> Build a blog for my site using the blog-builder skill.
> Brand color is #0e7490, font is Inter, language is Vietnamese.
```

Works the same for any agent that reads the [Agent Skills](https://agentskills.io) standard (`SKILL.md` with YAML frontmatter). For other tools, just point the agent at `skills/blog-builder/SKILL.md`.

## Design principles

1. **Token-first, brand-agnostic.** The standard defines structure and quality bars; *your* brand supplies color/type. No hardcoded palette.
2. **Reading UX is the product.** Auto-generated 2-level TOC with scroll-spy, reading progress, 65–75ch prose, styled bare-tag tables — because article HTML from a CMS has no classes.
3. **SEO/GEO 2026, not 2020.** Answer-first sections and TL;DR blocks (what AI engines actually cite), `Article` + `Author` + `Organization` + `Breadcrumb` JSON-LD, `dateModified` freshness. No fake ratings, no dead rich-result chasing.
4. **Responsive is non-negotiable.** Explicit breakpoint tiers; most readers are on mobile.
5. **Product-led, ad-free.** CTA slots promote *your own* product/service only.

## Prior art (researched)

No existing repo covers "blog UI standard as an agent skill" — this kit fills that niche. Format and conventions were learned from:

- [anthropics/skills](https://github.com/anthropics/skills) — official Agent Skills format (`SKILL.md` + frontmatter, progressive disclosure)
- [rampstackco/claude-skills](https://github.com/rampstackco/claude-skills) — skill conventions: When to use / When NOT / references under `references/`, stack-agnostic voice
- [bergside/awesome-design-skills](https://github.com/bergside/awesome-design-skills) — 67 DESIGN.md/SKILL.md style files (none blog-specific)
- [thedaviddias/Front-End-Checklist](https://github.com/thedaviddias/Front-End-Checklist) — checklist discipline for humans and AI agents
- [jiji262/claude-design-skill](https://github.com/jiji262/claude-design-skill) — portable single-skill design packaging

## License

MIT
