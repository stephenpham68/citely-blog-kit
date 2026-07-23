# Responsive — explicit breakpoint tiers

Most blog readers are on mobile (often 90%). Responsive is part of the build, not a follow-up. Use these tiers as `@media (max-width: …)` overrides on a desktop-first base (or invert if the stack is mobile-first — the *behaviors* are what matter).

## Article page

| Tier | Behavior |
|---|---|
| ≤ 1000px | Collapse the 3-column grid to one column. **Hide both rails.** Share rail → horizontal button row in the flow (sharing must survive). TOC → collapsed `<details>` "📑 In this article" above content, built from the same H2/H3 outline, same scroll-spy. |
| ≤ 640px | Header search → icon only. Cover → 16:9. Prose 17px. Related cards ~78–85vw with next-card peek. Stat/CTA rows go full-width. |
| ≤ 400px | Compact chips/badges; trim secondary labels. |

## Blog hub

| Tier | Behavior |
|---|---|
| ≤ 1024px | Spotlight stacks (most-read rail below featured). Card grids 3 → 2 columns. |
| ≤ 760px | Header condenses (search → icon). Hero + closing band padding shrinks. |
| ≤ 600px | Card grids → 1 column. Hero CTAs full-width. |
| ≤ 440px | Compact buttons; hide decorative badges. |

## Non-negotiables (all tiers)

- **Zero horizontal overflow.** Verify at 390px and 820px: `document.body.scrollWidth ≤ window.innerWidth`. Usual culprits: unwrapped tables, fixed-width images, long unbroken URLs (`overflow-wrap: break-word` on prose).
- Wide content (tables, code) scrolls **inside its own** `overflow-x: auto` wrapper.
- Tap targets ≥ 44×44px (share buttons, TOC links, stars, slider arrows).
- Sticky elements (header, TOC) never cover anchored headings — keep `scroll-margin-top` in sync with header height.
- Font sizes use `clamp()`; nothing below 13px for meaningful text.
- Test real gesture flows on mobile width: open mobile TOC and jump; share; swipe the related slider.
