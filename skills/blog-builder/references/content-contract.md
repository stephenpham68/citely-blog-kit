# Content contract — rendering article HTML from a CMS / Citely Connect

Articles typically arrive as **sanitized bare-tag HTML** plus metadata — no CSS classes, no inline styles. Your blog must make that raw HTML beautiful. This is why the prose spec in `article-page.md` targets bare tags.

## Typical payload fields (Citely Connect and similar)

| Field | Use |
|---|---|
| `title` | H1 + `<title>` + OG |
| `slug` | URL path `/blog/{slug}` |
| `html` (or `content`) | Article body — bare tags: `h2 h3 p a strong em ul ol li table thead tbody tr th td img figure figcaption blockquote` |
| `excerpt` / `description` | Meta description, card excerpts |
| `category`, `tags` | Badge, breadcrumb, hub sections |
| `author` (name, role, avatar, bio) | Byline + author box + `Person` schema |
| `publishedAt`, `updatedAt` | `<time>`, `datePublished` / `dateModified`, updated note |
| `coverImage` / `ogImage` | Cover + OG; gradient fallback when absent |
| `jsonLd` (sometimes) | If the source ships schema, merge — don't emit duplicates |

Citely Connect specifics: content is delivered via a signed-webhook envelope to your endpoint (see your Citely dashboard → Websites → Connect for the handshake). Store the payload; render with this standard.

## Rendering rules

1. **Sanitize on ingest** (allowlist tags/attrs) if the source isn't fully trusted; render server-side or via your framework's raw-HTML mechanism afterward.
2. **Post-process the HTML at render time** (server-side or build-time preferred):
   - Assign slug `id`s to every H2/H3 (deduplicate; strip diacritics for ASCII anchors) → feeds the auto-TOC.
   - Wrap every `<table>` in `<div class="table-wrap" style="overflow-x:auto">`.
   - Add `loading="lazy"` + `decoding="async"` to images below the fold.
   - **Strip manually-authored inline TOCs** (a list of same-page anchors near the top) — the auto-TOC replaces them; duplicates confuse readers.
   - External links: `rel="noopener"` (+ `nofollow`/`sponsored` per site policy).
3. **Never mutate wording.** You style and augment structure; you don't rewrite content.
4. **Missing fields degrade gracefully:** no cover → gradient + watermark; no author avatar → initials disc; no `updatedAt` (or same day as publish) → hide the updated note.
5. **Multi-article surfaces** (hub, related) consume the same metadata — one source of truth, no per-surface re-fetching of content bodies.
