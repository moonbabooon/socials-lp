# YASC Canada Social Landing Page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single `index.html` mobile-first landing page for the YASC Canada event, featuring an animated coin carousel of Yardi's social media accounts with a follow CTA panel.

**Architecture:** One self-contained `index.html` — all CSS and JS inline, zero external dependencies. The carousel is driven by a small vanilla JS state machine (`currentIndex`) that re-renders coin positions on every rotate. The follow panel is a fixed overlay toggled by CSS class.

**Tech Stack:** HTML5, CSS3 (custom properties, keyframe animations), vanilla ES6 JS. No build step. Deployed to GitHub Pages via `git push`.

---

## File Structure

| File | Purpose |
|------|---------|
| `index.html` | Complete page — markup, styles, scripts |

---

### Task 1: HTML skeleton + meta

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create `index.html` with full document shell**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#003087">
  <title>Follow Yardi on Social Media — #YASCCanada</title>
  <style>
    /* styles go here in Task 2 */
  </style>
</head>
<body>

  <!-- HEADER -->
  <header class="page-header">
    <div class="logo-pill">YARDI</div>
    <div class="hashtag">#YASCCanada</div>
    <h1 class="headline">Stay Connected With Us</h1>
    <p class="body-copy">See event updates, announcements and highlights! Follow us on LinkedIn, Facebook, Instagram and X, and don't forget to tag us and use the official hashtag when sharing your YASC moments.</p>
    <div class="divider"></div>
  </header>

  <!-- CAROUSEL -->
  <section class="carousel-section" aria-label="Social media accounts">
    <button class="nav-btn" id="btn-left" aria-label="Previous">&#8592;</button>
    <div class="coins-track" id="coins-track" role="list"></div>
    <button class="nav-btn" id="btn-right" aria-label="Next">&#8594;</button>
  </section>
  <div class="platform-label" id="platform-label" aria-live="polite"></div>
  <div class="swipe-hint">tap centre to follow &nbsp;·&nbsp; arrows to browse</div>

  <!-- FOLLOW PANEL -->
  <div class="panel-backdrop" id="panel-backdrop" aria-hidden="true"></div>
  <div class="follow-panel" id="follow-panel" role="dialog" aria-modal="true" aria-hidden="true">
    <div class="panel-icon" id="panel-icon"></div>
    <div class="panel-name" id="panel-name"></div>
    <div class="panel-handle" id="panel-handle"></div>
    <a class="panel-btn" id="panel-btn" target="_blank" rel="noopener noreferrer"></a>
    <div class="panel-dismiss">tap anywhere to close</div>
  </div>

  <script>
    /* scripts go here in Task 3+ */
  </script>

</body>
</html>
```

- [ ] **Step 2: Open `index.html` in a browser and verify the raw unstyled markup renders — all text visible, no JS errors in console**

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add HTML skeleton"
```

---

### Task 2: CSS — layout, header, coins, panel

**Files:**
- Modify: `index.html` — fill in the `<style>` block

- [ ] **Step 1: Add full CSS inside the `<style>` tag**

```css
/* ── Reset & base ── */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html {
  height: 100%;
  -webkit-tap-highlight-color: transparent;
}

body {
  min-height: 100%;
  background: linear-gradient(170deg, #f5f7fa 0%, #eaecf5 100%);
  font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 28px 20px 36px;
  gap: 0;
}

/* ── Header ── */
.page-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 360px;
  margin-bottom: 20px;
}

.logo-pill {
  font-size: 0.75rem;
  font-weight: 900;
  letter-spacing: 0.15em;
  color: #fff;
  background: #003087;
  padding: 5px 16px;
  border-radius: 5px;
  margin-bottom: 16px;
}

.hashtag {
  font-size: 1.1rem;
  font-weight: 900;
  color: #003087;
  letter-spacing: 0.04em;
  text-align: center;
  margin-bottom: 5px;
}

.headline {
  font-size: 0.88rem;
  font-weight: 800;
  color: #001a4d;
  letter-spacing: 0.07em;
  text-align: center;
  text-transform: uppercase;
  margin-bottom: 10px;
}

.body-copy {
  font-size: 0.67rem;
  color: #4a6080;
  text-align: center;
  line-height: 1.6;
  max-width: 280px;
  margin-bottom: 18px;
}

.divider {
  width: 36px;
  height: 2px;
  background: #003087;
  border-radius: 2px;
  opacity: 0.25;
}

/* ── Carousel ── */
.carousel-section {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  width: 100%;
  max-width: 360px;
  margin-bottom: 12px;
}

.nav-btn {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: #fff;
  border: 1.5px solid #c8d4e8;
  box-shadow: 0 2px 8px rgba(0, 48, 135, 0.12);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  flex-shrink: 0;
  font-size: 1rem;
  font-weight: 700;
  color: #003087;
  transition: background 0.15s, box-shadow 0.15s, transform 0.1s;
  user-select: none;
  -webkit-user-select: none;
}

.nav-btn:hover {
  background: #003087;
  color: #fff;
  box-shadow: 0 4px 14px rgba(0, 48, 135, 0.3);
  transform: scale(1.08);
}

.nav-btn:active { transform: scale(0.95); }

.coins-track {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  flex: 1;
  height: 96px;
}

/* ── Coins ── */
.coin {
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  position: relative;
  cursor: pointer;
  transition:
    transform 0.35s cubic-bezier(.34, 1.4, .64, 1),
    box-shadow 0.35s ease,
    opacity 0.35s ease;
}

.coin svg { display: block; pointer-events: none; }

/* Centre */
.coin-center {
  width: 72px;
  height: 72px;
  background: #fff;
  box-shadow: 0 6px 22px rgba(0, 48, 135, 0.22), 0 0 0 2px #003087;
  z-index: 3;
  animation: bob 2.6s ease-in-out infinite;
}

.coin-center:hover {
  box-shadow: 0 12px 30px rgba(0, 48, 135, 0.32), 0 0 0 2px #003087;
  animation-play-state: paused;
  transform: translateY(-6px) scale(1.06);
}

/* Pulse ring */
.coin-center::after {
  content: '';
  position: absolute;
  inset: -8px;
  border-radius: 50%;
  border: 2px solid #003087;
  opacity: 0;
  animation: pulse-ring 2.6s ease-out infinite;
  pointer-events: none;
}

/* Near (±1) */
.coin-near {
  width: 52px;
  height: 52px;
  background: #fff;
  box-shadow: 0 3px 10px rgba(0, 48, 135, 0.1), 0 0 0 1.5px #d0d8ec;
  opacity: 0.78;
  transform: scale(0.88);
  z-index: 2;
  animation: bob-near 2.6s ease-in-out infinite;
  animation-delay: 0.15s;
}

.coin-near:hover {
  opacity: 0.96;
  transform: scale(0.94) translateY(-3px);
  animation-play-state: paused;
}

/* Far (±2) */
.coin-far {
  width: 38px;
  height: 38px;
  background: #fff;
  box-shadow: 0 2px 6px rgba(0, 48, 135, 0.06), 0 0 0 1px #dde4f0;
  opacity: 0.35;
  transform: scale(0.72);
  z-index: 1;
  pointer-events: none;
}

/* SVG sizing per coin type */
.coin-center svg { width: 40px; height: 40px; }
.coin-near svg   { width: 29px; height: 29px; }
.coin-far svg    { width: 21px; height: 21px; }

/* ── Animations ── */
@keyframes bob {
  0%, 100% { transform: translateY(0); }
  50%       { transform: translateY(-8px); }
}

@keyframes bob-near {
  0%, 100% { transform: scale(0.88) translateY(0); }
  50%       { transform: scale(0.88) translateY(-4px); }
}

@keyframes pulse-ring {
  0%   { opacity: 0.5; transform: scale(0.9); }
  70%  { opacity: 0;   transform: scale(1.28); }
  100% { opacity: 0;   transform: scale(1.28); }
}

/* ── Labels ── */
.platform-label {
  font-size: 0.74rem;
  font-weight: 800;
  color: #003087;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  text-align: center;
  margin-bottom: 4px;
  min-height: 1.2em;
}

.swipe-hint {
  font-size: 0.58rem;
  color: #aabbd0;
  text-align: center;
  letter-spacing: 0.02em;
}

/* ── Follow panel ── */
.panel-backdrop {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0, 24, 70, 0.45);
  z-index: 10;
  animation: fade-in 0.2s ease;
}

.panel-backdrop.active { display: block; }

.follow-panel {
  display: none;
  position: fixed;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%) translateY(20px);
  width: min(360px, 100vw);
  background: linear-gradient(160deg, #002e80, #001a50);
  border-radius: 24px 24px 0 0;
  padding: 28px 24px 36px;
  z-index: 11;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  opacity: 0;
  transition: transform 0.32s cubic-bezier(.34, 1.3, .64, 1), opacity 0.25s ease;
}

.follow-panel.active {
  display: flex;
  opacity: 1;
  transform: translateX(-50%) translateY(0);
}

.panel-icon svg { width: 60px; height: 60px; display: block; }

.panel-name {
  font-size: 1.1rem;
  font-weight: 800;
  color: #fff;
  letter-spacing: 0.04em;
}

.panel-handle {
  font-size: 0.72rem;
  color: #7799bb;
  letter-spacing: 0.02em;
}

.panel-btn {
  display: inline-block;
  background: #003087;
  color: #fff;
  border: none;
  border-radius: 24px;
  padding: 10px 28px;
  font-size: 0.82rem;
  font-weight: 700;
  letter-spacing: 0.04em;
  text-decoration: none;
  cursor: pointer;
  margin-top: 6px;
  transition: background 0.15s, transform 0.1s, box-shadow 0.15s;
  box-shadow: 0 4px 16px rgba(0, 48, 135, 0.4);
}

.panel-btn:hover {
  background: #0044bb;
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(0, 48, 135, 0.55);
}

.panel-dismiss {
  font-size: 0.6rem;
  color: #445566;
  margin-top: 4px;
}

/* ── Fade in ── */
@keyframes fade-in {
  from { opacity: 0; }
  to   { opacity: 1; }
}

/* ── Reduced motion ── */
@media (prefers-reduced-motion: reduce) {
  .coin-center,
  .coin-near,
  .coin-center::after {
    animation: none;
  }
  .coin, .nav-btn, .panel-btn, .follow-panel {
    transition: none;
  }
}
```

- [ ] **Step 2: Open in browser — verify layout looks correct: logo pill, hashtag, headline, body copy, two arrow buttons, platform label, swipe hint. No overflow, no broken layout.**

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add CSS layout and animations"
```

---

### Task 3: Platform data + SVG icons

**Files:**
- Modify: `index.html` — add JS data array inside `<script>`

- [ ] **Step 1: Add platform data with inline SVG icons inside the `<script>` tag**

```js
const PLATFORMS = [
  {
    name: 'LinkedIn',
    handle: 'Yardi',
    url: 'https://www.linkedin.com/company/yardi/posts/?feedView=all',
    svg: `<svg viewBox="0 0 40 40" xmlns="http://www.w3.org/2000/svg">
      <rect width="40" height="40" rx="20" fill="#0A66C2"/>
      <rect x="10" y="17" width="5" height="13" fill="white"/>
      <circle cx="12.5" cy="12.5" r="2.8" fill="white"/>
      <path d="M18 17h4.8v1.8c.7-1.1 2.1-2.1 4.2-2.1 4.2 0 5 2.8 5 6.4V30h-5v-6c0-1.4 0-3.3-2-3.3s-2.3 1.6-2.3 3.2V30H18V17z" fill="white"/>
    </svg>`
  },
  {
    name: 'Facebook',
    handle: 'Yardi',
    url: 'https://www.facebook.com/Yardi/',
    svg: `<svg viewBox="0 0 40 40" xmlns="http://www.w3.org/2000/svg">
      <rect width="40" height="40" rx="20" fill="#1877F2"/>
      <path d="M22.5 21h3.2l.6-4H22.5v-2.3c0-1.1.3-1.9 2-1.9H26.5V9.2C26 9.1 24.6 9 23 9c-3.2 0-5.4 2-5.4 5.5V17H14v4h3.6v10h4.9V21z" fill="white"/>
    </svg>`
  },
  {
    name: 'Instagram',
    handle: '@yardisystems',
    url: 'https://www.instagram.com/yardisystems/?hl=en',
    svg: `<svg viewBox="0 0 40 40" xmlns="http://www.w3.org/2000/svg">
      <defs>
        <radialGradient id="ig" cx="30%" cy="107%" r="130%">
          <stop offset="0%"   stop-color="#ffd600"/>
          <stop offset="30%"  stop-color="#ff6d00"/>
          <stop offset="60%"  stop-color="#e1306c"/>
          <stop offset="80%"  stop-color="#833ab4"/>
          <stop offset="100%" stop-color="#405de6"/>
        </radialGradient>
      </defs>
      <rect width="40" height="40" rx="20" fill="url(#ig)"/>
      <rect x="10" y="10" width="20" height="20" rx="6" stroke="white" stroke-width="2.2" fill="none"/>
      <circle cx="20" cy="20" r="5" stroke="white" stroke-width="2.2" fill="none"/>
      <circle cx="27.2" cy="12.8" r="1.4" fill="white"/>
    </svg>`
  },
  {
    name: 'X',
    handle: '@Yardi',
    url: 'https://x.com/Yardi',
    svg: `<svg viewBox="0 0 40 40" xmlns="http://www.w3.org/2000/svg">
      <rect width="40" height="40" rx="20" fill="#000"/>
      <path d="M22.05 18.8 28.6 11h-1.58l-5.63 6.55L17.04 11H11.4l6.9 10.04L11.4 29h1.58l6.03-7.01L23.96 29h5.64l-7.55-10.2zm-2.13 2.48-.7-.99-5.55-7.95H16.3l4.48 6.41.7.99 5.83 8.34H24.7l-4.77-6.8z" fill="white"/>
    </svg>`
  }
];
```

- [ ] **Step 2: Verify no errors in browser console after adding the data**

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add platform data with SVG icons"
```

---

### Task 4: Carousel render + arrow navigation

**Files:**
- Modify: `index.html` — add carousel state machine and render function after `PLATFORMS`

- [ ] **Step 1: Add state, render function, and arrow listeners after the `PLATFORMS` array**

```js
let current = 0;

function renderCarousel() {
  const track = document.getElementById('coins-track');
  track.innerHTML = '';

  // Positions: -2 (far-left), -1 (near-left), 0 (center), +1 (near-right), +2 (far-right)
  const offsets = [-2, -1, 0, 1, 2];

  offsets.forEach(offset => {
    const idx = ((current + offset) % PLATFORMS.length + PLATFORMS.length) % PLATFORMS.length;
    const platform = PLATFORMS[idx];

    const coin = document.createElement('div');
    coin.setAttribute('role', 'listitem');

    if (offset === 0) {
      coin.className = 'coin coin-center';
      coin.setAttribute('aria-label', `${platform.name} — tap to follow`);
      coin.addEventListener('click', () => openPanel(idx));
    } else if (Math.abs(offset) === 1) {
      coin.className = 'coin coin-near';
      coin.setAttribute('aria-label', `Go to ${platform.name}`);
      coin.addEventListener('click', () => {
        current = idx;
        renderCarousel();
      });
    } else {
      coin.className = 'coin coin-far';
      coin.setAttribute('aria-hidden', 'true');
    }

    coin.innerHTML = platform.svg;
    track.appendChild(coin);
  });

  document.getElementById('platform-label').textContent = PLATFORMS[current].name;
}

document.getElementById('btn-left').addEventListener('click', () => {
  current = (current - 1 + PLATFORMS.length) % PLATFORMS.length;
  renderCarousel();
});

document.getElementById('btn-right').addEventListener('click', () => {
  current = (current + 1) % PLATFORMS.length;
  renderCarousel();
});

renderCarousel();
```

- [ ] **Step 2: Open in browser — verify coins render, arrows rotate platforms, platform label updates, near coins rotate when clicked**

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: carousel render and arrow navigation"
```

---

### Task 5: Follow panel — open, populate, close

**Files:**
- Modify: `index.html` — add `openPanel` and `closePanel` functions

- [ ] **Step 1: Add panel functions after `renderCarousel()`**

```js
function openPanel(idx) {
  const platform = PLATFORMS[idx];
  document.getElementById('panel-icon').innerHTML = platform.svg;
  document.getElementById('panel-name').textContent = platform.name;
  document.getElementById('panel-handle').textContent = platform.handle;

  const btn = document.getElementById('panel-btn');
  btn.textContent = `Follow us on ${platform.name} →`;
  btn.href = platform.url;

  const panel = document.getElementById('follow-panel');
  const backdrop = document.getElementById('panel-backdrop');

  panel.removeAttribute('aria-hidden');
  backdrop.removeAttribute('aria-hidden');

  // Force reflow so CSS transition fires
  panel.style.display = 'flex';
  backdrop.classList.add('active');
  requestAnimationFrame(() => {
    requestAnimationFrame(() => {
      panel.classList.add('active');
    });
  });
}

function closePanel() {
  const panel = document.getElementById('follow-panel');
  const backdrop = document.getElementById('panel-backdrop');

  panel.classList.remove('active');
  backdrop.classList.remove('active');
  panel.setAttribute('aria-hidden', 'true');
  backdrop.setAttribute('aria-hidden', 'true');

  setTimeout(() => {
    panel.style.display = 'none';
  }, 320);
}

document.getElementById('panel-backdrop').addEventListener('click', closePanel);
```

- [ ] **Step 2: Open in browser — tap center coin, verify panel slides up with correct platform name/handle/button. Tap backdrop, verify panel closes. Rotate to each platform and verify panel populates correctly for all 4.**

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: follow panel open/close with platform data"
```

---

### Task 6: Touch swipe support

**Files:**
- Modify: `index.html` — add touch event listeners after `closePanel`

- [ ] **Step 1: Add swipe detection on the `coins-track` element**

```js
(function attachSwipe() {
  const track = document.getElementById('coins-track');
  let startX = null;

  track.addEventListener('touchstart', e => {
    startX = e.touches[0].clientX;
  }, { passive: true });

  track.addEventListener('touchend', e => {
    if (startX === null) return;
    const dx = e.changedTouches[0].clientX - startX;
    startX = null;

    if (Math.abs(dx) < 40) return; // ignore tiny taps

    if (dx < 0) {
      // swipe left → next
      current = (current + 1) % PLATFORMS.length;
    } else {
      // swipe right → previous
      current = (current - 1 + PLATFORMS.length) % PLATFORMS.length;
    }
    renderCarousel();
  }, { passive: true });
})();
```

- [ ] **Step 2: Test on mobile or using browser DevTools device emulation (iPhone SE or similar) — swipe left and right to verify carousel rotates. Confirm short taps on center coin still open the panel (dx < 40 guard).**

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: touch swipe navigation"
```

---

### Task 7: Polish — panel SVG sizing + iOS safe area

**Files:**
- Modify: `index.html` — small CSS additions

- [ ] **Step 1: Add panel SVG size rule and iOS safe-area padding to the CSS**

```css
/* Inside the existing <style> block, append: */

/* Panel icon size */
.panel-icon svg {
  width: 64px;
  height: 64px;
  display: block;
}

/* iOS notch / home indicator safe area */
.follow-panel {
  padding-bottom: max(36px, env(safe-area-inset-bottom));
}
```

- [ ] **Step 2: Open in browser — verify panel icon is large and clear. If on iOS (or simulated), verify panel clears the home indicator bar.**

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: panel icon size and iOS safe area"
```

---

### Task 8: Push to GitHub

**Files:**
- None created — git remote setup and push

- [ ] **Step 1: Add the GitHub remote**

```bash
git remote add origin https://github.com/moonbabooon/socials-lp.git
```

- [ ] **Step 2: Push**

```bash
git push -u origin master
```

Expected output: branch pushed, `master` tracking `origin/master`.

- [ ] **Step 3: Enable GitHub Pages in the repo settings**

Go to `https://github.com/moonbabooon/socials-lp/settings/pages`, set Source to `Deploy from a branch`, branch `master`, folder `/ (root)`. The page will be live at `https://moonbabooon.github.io/socials-lp/`.

- [ ] **Step 4: Verify live URL loads correctly on a real mobile device**

Open `https://moonbabooon.github.io/socials-lp/` on a phone. Check: layout fits screen, coins animate, arrows work, panel opens and links out correctly.

---

## Self-Review

**Spec coverage:**
- ✅ Single HTML file, no dependencies
- ✅ Yardi navy `#003087` colour scheme, light background
- ✅ Header: logo pill, `#YASCCanada`, headline, body copy, divider
- ✅ 4 platforms: LinkedIn, Facebook, Instagram, X with correct URLs
- ✅ Arc depth: centre (72px), near (52px), far (38px)
- ✅ Real brand SVG icons with correct brand colours
- ✅ Bob animation on centre + near coins
- ✅ Pulse ring on centre coin
- ✅ Left + right nav arrows, equal size
- ✅ Near coin click rotates carousel
- ✅ Touch swipe navigation
- ✅ Follow CTA panel: icon, name, handle, button, dismiss
- ✅ `prefers-reduced-motion` disables all animations
- ✅ Links open in `_blank` with `rel="noopener noreferrer"`
- ✅ Push to `https://github.com/moonbabooon/socials-lp.git`

**Handles per platform:** LinkedIn → `Yardi`, Facebook → `Yardi`, Instagram → `@yardisystems`, X → `@Yardi`
