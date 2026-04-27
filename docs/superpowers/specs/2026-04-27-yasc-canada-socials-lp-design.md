# YASC Canada Social Media Landing Page — Design Spec

**Date:** 2026-04-27  
**Output:** Single HTML file (`index.html`)  
**Repo:** https://github.com/moonbabooon/socials-lp.git  
**Use case:** QR code destination at the YASC Canada event — attendees scan and are taken to this page to follow Yardi on social media.

---

## Overview

A mobile-first, single-page landing page that presents Yardi's social media accounts as an interactive coin carousel. Inspired by fighting game character selectors — icons orbit horizontally, the active one is centred and enlarged, side ones recede. Tapping the centre coin reveals a follow CTA panel.

---

## Visual Design

**Palette**
- Background: `linear-gradient(170deg, #f5f7fa, #eaecf5)` — light, clean
- Primary: `#003087` (Yardi navy)
- Text dark: `#001a4d`
- Text muted: `#4a6080`
- Coin ring / accent: `#003087`

**Typography**
- Font: `'Segoe UI', system-ui, sans-serif` — no external font load
- Hashtag: 1rem, weight 900, navy
- Headline: 0.82rem, weight 800, uppercase, navy dark
- Body: 0.62rem, weight 400, muted

**Logo**
- "YARDI" text pill — white text on `#003087` background, bold, letter-spaced

---

## Page Structure (top to bottom)

1. **Header block**
   - YARDI logo pill (centred)
   - `#YASCCanada` (navy, bold)
   - `STAY CONNECTED WITH US` headline
   - Body copy: *"See event updates, announcements and highlights! Follow us on LinkedIn, Facebook, Instagram and X, and don't forget to tag us and use the official hashtag when sharing your YASC moments."*
   - Thin navy divider rule

2. **Carousel**
   - Left arrow button · coins track · Right arrow button
   - Platform name label below coins
   - Swipe hint: *"tap centre to follow · arrows to browse"*

3. **Follow CTA panel** (overlay, slides up on coin tap)

---

## Carousel

**Platforms (fixed order, wraps):**
| # | Platform | URL |
|---|----------|-----|
| 1 | LinkedIn | https://www.linkedin.com/company/yardi/posts/?feedView=all |
| 2 | Facebook | https://www.facebook.com/Yardi/ |
| 3 | Instagram | https://www.instagram.com/yardisystems/?hl=en |
| 4 | X | https://x.com/Yardi |

**Coin sizes (arc depth):**
| Position | Size | Opacity | Scale |
|----------|------|---------|-------|
| Centre | 68px | 100% | 1.0 |
| Near (±1) | 50px | 78% | 0.88 |
| Far (±2) | 36px | 38% | 0.72 |

**Coin icons:** Inline SVG with real brand colours (LinkedIn `#0A66C2`, Facebook `#1877F2`, Instagram gradient, X `#000000`).

**Navigation:**
- Left/right arrow buttons (36px circles, navy on hover)
- Touch swipe (left/right) — essential for mobile
- Clicking a near coin rotates to that platform
- Clicking the far coin does nothing (too small, purely decorative)

---

## Follow CTA Panel

Triggered by tapping the centre coin. Slides up from below the carousel.

**Contents:**
- Platform logo (large, centred, inline SVG)
- Platform name (e.g. "LinkedIn")
- Yardi handle per platform:
  - LinkedIn: `Yardi`
  - Facebook: `Yardi`
  - Instagram: `@yardisystems`
  - X: `@Yardi`
- CTA button: `Follow us on [Platform] →` — opens URL in new tab, navy pill button
- Dismiss hint: `"tap anywhere to close"`

**Dismiss:** tap anywhere outside the panel.

---

## Animations

| Element | Animation | Details |
|---------|-----------|---------|
| Centre coin | Bob (float) | `translateY(0 → -7px)`, 2.6s ease-in-out, infinite, pauses on hover |
| Near coins | Bob (small) | `translateY(0 → -4px)`, 2.6s, 0.15s delay |
| Centre coin | Pulse ring | Expanding border ring, 2.6s, opacity fade out |
| Arrow buttons | Hover | Navy fill, white arrow, `scale(1.08)` |
| Coins | Hover | Lift + slight scale, pointer cursor |
| Carousel rotate | Transition | `cubic-bezier(.34,1.4,.64,1)`, 0.35s |
| Follow panel | Enter | Slide up, spring ease |
| Follow panel backdrop | Enter | Dim fade |

All animations disabled when `prefers-reduced-motion: reduce` is set.

---

## Technical

- **Output:** Single `index.html` — no external dependencies, no build step
- **Hosting:** Push to `https://github.com/moonbabooon/socials-lp.git`, deploy via GitHub Pages
- **Links:** Open in `_blank` (new tab) so users stay on the landing page
- **Viewport:** `width=device-width, initial-scale=1.0`
- **Touch:** `touch-action` and passive listeners for smooth swipe on iOS/Android
