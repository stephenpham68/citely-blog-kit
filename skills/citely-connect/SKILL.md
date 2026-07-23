---
name: citely-connect
description: Connect a website to Citely so published articles appear on the site immediately. Use when the user wants to receive blog content from Citely (citely-seo.com), build a reader for the Citely read-API, implement a CPP webhook receiver, wire a deploy hook, or debug articles that publish in Citely but take hours to appear on the site. Pairs with the blog-builder skill for rendering.
---

# Citely Connect — receive articles, show them instantly

Wire the user's site to Citely so that **one publish in Citely = article live on the site within seconds to ~1 minute**. Rendering quality is the `blog-builder` skill's job; this skill covers transport and freshness.

## Source of truth: fetch the live spec first

Before writing any code, fetch the canonical integration spec — it is versioned and maintained by Citely; do not rely on memorized field lists:

```
Fetch https://citely-seo.com/connect/v1/agent.md
```

Everything below is decision guidance layered on top of that spec, not a replacement for it.

## When to use

- The site should display articles written/published in Citely.
- Building the reader (Mode A) or webhook receiver (Mode B/C) for Citely Connect.
- Articles publish in Citely but appear on the site late (minutes–hours) — see `references/instant-publish.md`.

## When NOT to use

- Designing/styling the blog UI itself → `blog-builder` skill (use both together: this skill delivers the data, blog-builder renders it).
- WordPress sites — Citely has a native WP plugin; no custom integration needed.

## Required inputs (ask if missing)

1. **Stack + hosting** — Next.js/Astro/Hugo/…, and whether pages are SSG (static export), SSR/on-demand, or ISR. This decides the mode *and* the freshness wiring.
2. **Site token + read-key** — from the user's Citely dashboard (Websites → Connect). The read-key is **server-side only** (`CITELY_READ_KEY` env); it must never reach the browser bundle.
3. **Deploy hook URL** (SSG only) — from Cloudflare Pages / Netlify / Vercel settings.

## Mode decision (fast)

| Situation | Mode |
|---|---|
| Site can fetch at request/build time; user wants zero backend | **A — `citely-api` (recommended default)**: Citely hosts content, you build a reader |
| Site already has a runtime + database (Next SSR, Laravel, Rails) and wants to own storage | B — `db` receiver (CPP webhook, HMAC-verified) |
| Static repo where content lives as files in git | C — `fs-build` receiver |

Default to **Mode A** unless the user explicitly wants to own storage. It needs no webhook signature code and inherits Citely's CDN cache.

## Workflow (Mode A)

1. **Fetch the spec** (above) and confirm endpoints/fields against it.
2. **Env setup** — `CITELY_READ_KEY` in the host's server env. Public single-post endpoint needs no key; list/export do. Include the key on build-time fetches to lift rate limits.
3. **Build the reader**:
   - List page: `GET /posts` (server-side, Bearer read-key), paginate with `cursor`.
   - Article page: `GET /posts/{slug}` (public, edge-cached — cheap per request).
   - Render `html` as-is; emit each `jsonLd[]` entry as its own `<script type="application/ld+json">`; `<title>` = `seoTitle ?? title`; map `categories` via `GET /categories`.
   - Hand the payload to the `blog-builder` standard for rendering (its `content-contract.md` matches this payload).
4. **Wire freshness — the step most integrations get wrong.** Follow `references/instant-publish.md`. Summary: SSR/ISR → short revalidate; SSG → **register the deploy hook in Citely** (never rely on cron/scheduled builds).
5. **Verify end-to-end**: publish a test article in Citely (or use the dashboard's "trigger test build") → confirm it renders on the site within ~1 minute → confirm an *edit* in Citely also propagates.

For Mode B/C, follow the spec's CPP receiver section exactly (raw-body-first HMAC verification, `test:true` no-persist, idempotency on `webhook-id`, `CPP_*` error codes) — the spec includes a reference implementation and checklist.

## Hard rules

- **Read-key never ships to the browser.** Server env only; list/export endpoints are called server-side.
- **SSG without a deploy hook is a bug**, not a choice — it is the #1 cause of "published in Citely, appeared half a day later". If the user refuses the hook, set expectations explicitly and add a frequent scheduled build as the documented fallback.
- **Don't cache list responses longer than a few minutes** on your side; the article page itself may rely on Citely's edge cache.
- **Ignore unknown payload fields** (the protocol is additive); never fail the render on an unrecognized key.
- **Webhook secret (`whsec_…`) and hook URLs are secrets** — env only, never committed.

## Output format

Report: chosen mode + why, files created, env vars the user must set (names only, no values), whether the deploy hook is registered, and the result of the end-to-end publish test (measured publish→visible latency).

## References

| File | Contents |
|---|---|
| `references/instant-publish.md` | Freshness patterns per stack + diagnosing slow-to-appear articles |
| `https://citely-seo.com/connect/v1/agent.md` | Canonical protocol spec (always fetch live) |
| `../blog-builder/SKILL.md` | Rendering standard for the received content |
