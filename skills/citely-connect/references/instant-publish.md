# Instant publish — freshness patterns & the slow-article diagnosis

Goal: **publish in Citely → visible on the site in seconds to ~1 minute.** Citely batches rapid publishes (~45s window → one deploy-hook call), so end-to-end latency is dominated by *your* side. Pick the pattern for the stack:

## Patterns by rendering strategy

| Strategy | Wiring | Publish→visible |
|---|---|---|
| **SSR / on-demand** (Next SSR, Astro SSR, Laravel, Rails) | Fetch `GET /posts/{slug}` per request (public + edge-cached). No hook needed. | Seconds |
| **ISR** (Next.js) | `revalidate: 300` (or less) on article + list routes; or on-demand revalidation triggered by a tiny endpoint you point Citely's deploy hook at. | Seconds–minutes |
| **SSG** (Astro static, Next export, Hugo, Eleventy) | **Register the deploy hook in Citely** (Cloudflare Pages / Netlify / Vercel build hook URL). Citely POSTs it after every publish/edit/delete → site rebuilds automatically. | ~1 build time (1–3 min) |
| SSG, user refuses hook | Scheduled build — but state the tradeoff: hourly build = up to 1h delay. Never accept daily. | Up to schedule interval |

Extra freshness rules:

- **Scheduled ("future") posts** only appear at the first rebuild after `publishedAt` — SSG sites that use scheduling need a periodic build (e.g. hourly) *in addition to* the hook, or should avoid scheduled posts.
- **List caching:** if you cache `GET /posts` responses (KV, memory, HTTP cache), cap TTL at ~5 minutes. The most common self-inflicted delay is a list page cached for hours while the article page is already live.
- **CDN page cache:** if the site sits behind its own CDN page-caching (e.g. Cloudflare "cache everything"), purge on deploy or set short TTLs on `/blog*` HTML.

## Diagnosing "published in Citely, appeared hours later"

Work down this list — it's ordered by how often each is the culprit:

1. **SSG site with no deploy hook registered** → site only rebuilds when the owner deploys something else or a daily cron runs. Fix: paste the build-hook URL into Citely (dashboard → Websites → Connect → Deploy/Rebuild hook), then "trigger test build" to verify.
2. **Deploy hook registered but broken** — rotated/revoked URL, wrong project, or the host rejects the POST. Fix: fire the hook manually (`curl -X POST <hook-url>`) and watch the host's deploy log; re-paste a fresh hook if nothing triggers.
3. **List/blog-index cached long** on the site or its CDN → article page exists but never gets linked. Check: open the article by direct slug URL; if it loads while the index doesn't show it, it's a list-cache problem.
4. **ISR with a huge `revalidate`** (e.g. 86400) → cap at minutes or add on-demand revalidation.
5. **Scheduled post** (`status: "future"`) → it was never "late"; it appeared at the first rebuild after its publish time. Explain the semantics.
6. Only after all the above: suspect the Citely side (hook not firing) — verify with the dashboard's test-build button and the site's deploy history timestamps.

## Verification protocol (always run after wiring)

1. Publish a throwaway article in Citely; note the time.
2. Watch the host's deploy log (SSG) or hit the slug URL (SSR/ISR).
3. Record publish→visible latency; it should match the table above. Report the measured number to the user.
4. Edit the article's title in Citely; confirm the change propagates too (updates travel the same path).
5. Delete the throwaway; confirm it disappears (SSG: after the hook-triggered rebuild).
