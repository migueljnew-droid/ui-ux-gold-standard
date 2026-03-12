# GENESIS Gold Standard — Award-Winning Techniques Reference

Production-ready code patterns reverse-engineered from Awwwards SOTY winners and elite creative studios.
Every technique includes working code, performance notes, and common mistakes.

**Source sites studied:** landonorris.com, igloo.inc (SOTY 2024), messenger.abeto.co, dontboardme.com, kprverse.com, synchronized.studio, stitchingstories.nl, beckyentertainment.co, manayerbamate.com

---

## The Award-Winning Stack

| Layer | Tool | Why |
|-------|------|-----|
| **Smooth Scroll** | Lenis | Used by 4/9 award winners. Buttery 60fps scroll |
| **Animation** | GSAP + ScrollTrigger | Used by 5/9. Industry standard for scroll-linked animation |
| **3D** | Three.js (+ Threlte for Svelte) | Used by 3/9 (and the MOST awarded) |
| **Framework** | Nuxt.js (Vue) or Next.js (React) | 5/9 use Nuxt, strong SEO + SSR |
| **CMS** | Sanity / Storyblok / Contentful | 6/9 use headless CMS |
| **Text Splitting** | SplitType | Character/word/line animation |
| **Page Transitions** | Barba.js or custom | SPA-like transitions on MPA |
| **Build** | Vite | Fast HMR, tree-shaking |

### CDN Links (Latest Stable)
```html
<!-- GSAP + ScrollTrigger -->
<script src="https://cdn.jsdelivr.net/npm/gsap@3/dist/gsap.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3/dist/ScrollTrigger.min.js"></script>

<!-- Lenis -->
<script src="https://cdn.jsdelivr.net/npm/lenis@latest/dist/lenis.min.js"></script>

<!-- SplitType -->
<script src="https://cdn.jsdelivr.net/npm/split-type@0.3.4/umd/index.min.js"></script>

<!-- Barba.js -->
<script src="https://cdn.jsdelivr.net/npm/@barba/core@2/dist/barba.umd.js"></script>
```

### NPM Install
```bash
npm install gsap lenis split-type @barba/core three
```

---

## TECHNIQUE 1: Lenis + GSAP ScrollTrigger Foundation

The foundation of every award-winning scroll experience. Get this wrong and everything else falls apart.

### Vanilla JS Setup

```javascript
import Lenis from 'lenis';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

// Initialize Lenis
const lenis = new Lenis({
  duration: 1.2,
  easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
  smoothWheel: true,
  smoothTouch: false, // NEVER smooth touch — ruins mobile UX
  wheelMultiplier: 1,
  touchMultiplier: 2,
});

// CRITICAL: Sync Lenis with GSAP ticker
lenis.on('scroll', ScrollTrigger.update);

gsap.ticker.add((time) => {
  lenis.raf(time * 1000); // Convert seconds to milliseconds
});

gsap.ticker.lagSmoothing(0); // Disable lag smoothing for perfect sync
```

### Required CSS
```css
html.lenis, html.lenis body {
  height: auto;
}

.lenis.lenis-smooth {
  scroll-behavior: auto !important;
}

.lenis.lenis-smooth [data-lenis-prevent] {
  overscroll-behavior: contain;
}

.lenis.lenis-stopped {
  overflow: hidden;
}
```

### React/Next.js Setup
```jsx
'use client';
import { useEffect, useRef } from 'react';
import Lenis from 'lenis';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

export function SmoothScroll({ children }) {
  const lenisRef = useRef(null);

  useEffect(() => {
    const lenis = new Lenis({
      duration: 1.2,
      easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
      smoothWheel: true,
      smoothTouch: false,
    });

    lenisRef.current = lenis;

    lenis.on('scroll', ScrollTrigger.update);
    gsap.ticker.add((time) => lenis.raf(time * 1000));
    gsap.ticker.lagSmoothing(0);

    return () => {
      lenis.destroy();
      gsap.ticker.remove(lenis.raf);
    };
  }, []);

  return <>{children}</>;
}
```

### Accessibility: Reduced Motion
```javascript
const prefersReduced = window.matchMedia('(prefers-reduced-motion: reduce)');

if (prefersReduced.matches) {
  // Skip Lenis entirely — use native scroll
  gsap.set('[data-animate]', { clearProps: 'all' });
} else {
  // Initialize Lenis + animations
  const lenis = new Lenis({ duration: 1.2 });
  // ... setup code
}
```

### Common Bugs and Fixes

| Bug | Cause | Fix |
|-----|-------|-----|
| ScrollTrigger positions wrong | Lenis changes scroll behavior | Always call `ScrollTrigger.refresh()` after layout changes |
| Pin spacing broken | Lenis + pin conflict | Use `pinSpacing: true` (default) and `anticipatePin: 1` |
| Scroll position desyncs | autoRaf conflicts with GSAP ticker | NEVER use `autoRaf: true` with GSAP. Use manual ticker sync |
| Mobile scroll feels weird | `smoothTouch: true` | Set `smoothTouch: false` — always |
| Resize breaks triggers | No refresh on resize | `ScrollTrigger.addEventListener('refreshInit', () => lenis.resize())` |
| Nested scroll areas broken | Lenis intercepts all scroll | Add `data-lenis-prevent` attribute to nested scrollable elements |

---

## TECHNIQUE 2: Text Splitting Animations

Used by: landonorris.com, kprverse.com, synchronized.studio

### SplitType + GSAP (Production Pattern)

```javascript
import SplitType from 'split-type';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

// Split into lines, words, and characters
const text = new SplitType('.hero-heading', {
  types: 'lines, words, chars',
  tagName: 'span',
});

// Wrap each line in an overflow-hidden container
text.lines.forEach((line) => {
  const wrapper = document.createElement('div');
  wrapper.style.overflow = 'hidden';
  line.parentNode.insertBefore(wrapper, line);
  wrapper.appendChild(line);
});

// Staggered line reveal (Landon Norris style)
gsap.from(text.lines, {
  y: '100%',
  opacity: 0,
  duration: 0.8,
  ease: 'power3.out',
  stagger: 0.15,
  scrollTrigger: {
    trigger: '.hero-heading',
    start: 'top 80%',
    toggleActions: 'play none none reverse',
  },
});
```

### Clip-Path Line Reveal (No Library)
```css
.line-reveal {
  overflow: hidden;
}

.line-reveal span {
  display: block;
  clip-path: inset(100% 0 0 0);
  transition: clip-path 0.8s cubic-bezier(0.77, 0, 0.175, 1);
}

.line-reveal.visible span {
  clip-path: inset(0% 0 0 0);
}

.line-reveal span:nth-child(1) { transition-delay: 0s; }
.line-reveal span:nth-child(2) { transition-delay: 0.1s; }
.line-reveal span:nth-child(3) { transition-delay: 0.2s; }
```

### Per-Character Rotation
```javascript
gsap.from(text.chars, {
  opacity: 0,
  rotationY: 90,
  transformOrigin: 'center center',
  duration: 0.6,
  ease: 'back.out(1.7)',
  stagger: 0.03,
  scrollTrigger: {
    trigger: '.hero-heading',
    start: 'top 80%',
  },
});
```

### Resize Handling (CRITICAL)
```javascript
// SplitType breaks on resize — must re-split
let split;

function initSplit() {
  if (split) split.revert(); // Clean up old split
  split = new SplitType('.hero-heading', { types: 'lines, words, chars' });
  // Re-apply animations...
}

initSplit();
window.addEventListener('resize', debounce(initSplit, 300));
```

### Performance Notes
- SplitType is ~14KB gzipped — acceptable
- Each split character becomes a DOM node — avoid splitting paragraphs with 500+ characters
- Always use `transform` for animation, never `top/left`
- Add `will-change: transform` during animation, remove after

---

## TECHNIQUE 3: Custom Cursor with mix-blend-mode

Used by: dontboardme.com, kprverse.com, synchronized.studio

### Complete Implementation

```html
<div class="cursor" aria-hidden="true"></div>
<div class="cursor-follower" aria-hidden="true"></div>
```

```css
/* Hide on touch devices */
@media (hover: none) {
  .cursor, .cursor-follower { display: none !important; }
}

body { cursor: none; }

/* Restore cursor on touch */
@media (hover: none) {
  body { cursor: auto; }
}

.cursor {
  width: 20px;
  height: 20px;
  border: 2px solid #fff;
  border-radius: 50%;
  position: fixed;
  top: 0;
  left: 0;
  pointer-events: none;
  z-index: 9999;
  mix-blend-mode: difference;
  transform: translate(-50%, -50%);
  transition: width 0.3s, height 0.3s, border-color 0.3s;
  will-change: transform;
}

.cursor.hover {
  width: 60px;
  height: 60px;
  border-color: transparent;
  background: rgba(255, 255, 255, 0.1);
}

.cursor-follower {
  width: 8px;
  height: 8px;
  background: #fff;
  border-radius: 50%;
  position: fixed;
  top: 0;
  left: 0;
  pointer-events: none;
  z-index: 9998;
  mix-blend-mode: difference;
  transform: translate(-50%, -50%);
  will-change: transform;
}
```

```javascript
const cursor = document.querySelector('.cursor');
const follower = document.querySelector('.cursor-follower');

// Skip entirely on touch devices
if (!window.matchMedia('(hover: none)').matches) {
  let mouseX = 0, mouseY = 0;
  let followerX = 0, followerY = 0;

  document.addEventListener('mousemove', (e) => {
    mouseX = e.clientX;
    mouseY = e.clientY;
    cursor.style.left = mouseX + 'px';
    cursor.style.top = mouseY + 'px';
  });

  function lerp(start, end, factor) {
    return start + (end - start) * factor;
  }

  function animateFollower() {
    followerX = lerp(followerX, mouseX, 0.15);
    followerY = lerp(followerY, mouseY, 0.15);
    follower.style.left = followerX + 'px';
    follower.style.top = followerY + 'px';
    requestAnimationFrame(animateFollower);
  }
  animateFollower();

  // Context-aware cursor states
  document.querySelectorAll('a, button, [data-cursor="pointer"]').forEach((el) => {
    el.addEventListener('mouseenter', () => cursor.classList.add('hover'));
    el.addEventListener('mouseleave', () => cursor.classList.remove('hover'));
  });
}
```

### Magnetic Button Effect
```javascript
document.querySelectorAll('.magnetic-btn').forEach((btn) => {
  btn.addEventListener('mousemove', (e) => {
    const rect = btn.getBoundingClientRect();
    const x = e.clientX - rect.left - rect.width / 2;
    const y = e.clientY - rect.top - rect.height / 2;

    gsap.to(btn, { x: x * 0.3, y: y * 0.3, duration: 0.3, ease: 'power2.out' });
  });

  btn.addEventListener('mouseleave', () => {
    gsap.to(btn, { x: 0, y: 0, duration: 0.5, ease: 'elastic.out(1, 0.5)' });
  });
});
```

---

## TECHNIQUE 4: Clip-Path Scroll Reveals

Used by: landonorris.com, dontboardme.com, stitchingstories.nl

### Polygon Reveal (Bottom to Top)
```javascript
gsap.from('.reveal-image', {
  clipPath: 'polygon(0% 100%, 100% 100%, 100% 100%, 0% 100%)',
  duration: 1.2,
  ease: 'power2.out',
  scrollTrigger: {
    trigger: '.reveal-image',
    start: 'top 80%',
    end: 'top 30%',
    scrub: 0.5,
  },
});
// Target state: clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)
```

### Circle Expand from Center
```javascript
gsap.from('.circle-reveal', {
  clipPath: 'circle(0% at 50% 50%)',
  scrollTrigger: {
    trigger: '.circle-reveal',
    start: 'top 75%',
    end: 'top 25%',
    scrub: 1,
  },
});
```

### Ellipse Reveal (Landon Norris Style)
```javascript
gsap.from('.hero-image', {
  clipPath: 'ellipse(0% 0% at 50% 0%)',
  scrollTrigger: {
    trigger: '.hero-section',
    start: 'top top',
    end: 'bottom top',
    scrub: true,
  },
});
```

### Inset Reveal (Curtain Open)
```javascript
gsap.from('.inset-reveal', {
  clipPath: 'inset(0% 50% 0% 50%)',
  duration: 1,
  scrollTrigger: { trigger: '.inset-reveal', start: 'top 70%', scrub: 1 },
});
```

### Performance
- `clip-path` is GPU-accelerated in modern browsers — safe to animate
- Add `will-change: clip-path` during animation, remove after
- Avoid animating clip-path on elements larger than 2x viewport

---

## TECHNIQUE 5: Horizontal Scroll Sections

Used by: landonorris.com, stitchingstories.nl, synchronized.studio

### GSAP Pin + TranslateX Pattern
```html
<section class="horizontal-wrapper">
  <div class="horizontal-track">
    <div class="panel">Panel 1</div>
    <div class="panel">Panel 2</div>
    <div class="panel">Panel 3</div>
    <div class="panel">Panel 4</div>
  </div>
</section>
```

```css
.horizontal-wrapper { overflow: hidden; }
.horizontal-track { display: flex; width: max-content; }
.panel {
  width: 100vw;
  height: 100vh;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

```javascript
const track = document.querySelector('.horizontal-track');

gsap.to(track, {
  x: () => -(track.scrollWidth - window.innerWidth),
  ease: 'none',
  scrollTrigger: {
    trigger: '.horizontal-wrapper',
    pin: true,
    scrub: 1,
    end: () => '+=' + (track.scrollWidth - window.innerWidth),
    invalidateOnRefresh: true,
  },
});
```

### Mobile Fallback
```javascript
ScrollTrigger.matchMedia({
  '(min-width: 769px)': function () {
    gsap.to(track, {
      x: () => -(track.scrollWidth - window.innerWidth),
      scrollTrigger: { trigger: '.horizontal-wrapper', pin: true, scrub: 1 },
    });
  },
  '(max-width: 768px)': function () {
    track.style.flexDirection = 'column';
    document.querySelectorAll('.panel').forEach((p) => {
      p.style.width = '100%';
      p.style.minHeight = '100vh';
    });
  },
});
```

---

## TECHNIQUE 6: Gooey / Liquid Filter Effect

Used by: kprverse.com

### CSS blur + contrast Trick
```css
.gooey-container {
  filter: blur(40px) contrast(20);
  background: #000; /* MUST have opaque background for contrast to work */
}

.blob {
  position: absolute;
  border-radius: 50%;
  background: #667eea;
}
```

The trick: high `contrast()` sharpens blurred edges, creating a liquid merge where shapes overlap.

### SVG Filter (More Control)
```html
<svg style="position: absolute; width: 0; height: 0;">
  <defs>
    <filter id="goo">
      <feGaussianBlur in="SourceGraphic" stdDeviation="10" result="blur" />
      <feColorMatrix in="blur" mode="matrix"
        values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -7" result="goo" />
      <feComposite in="SourceGraphic" in2="goo" operator="atop" />
    </filter>
  </defs>
</svg>

<div style="filter: url(#goo)">
  <!-- blobs here -->
</div>
```

The magic: `feColorMatrix values="... 18 -7"` — the `18` sharpens alpha, `-7` clips the threshold.

---

## TECHNIQUE 7: Variable Font Animations

Used by: landonorris.com (Brier), kprverse.com (ABC Whyte Plus)

### CSS @property for Smooth Interpolation
```css
@property --wght {
  syntax: '<number>';
  initial-value: 400;
  inherits: false;
}

.variable-heading {
  font-variation-settings: 'wght' var(--wght);
  transition: --wght 0.4s ease;
}

.variable-heading:hover {
  --wght: 900;
}
```

### Scroll-Linked Weight Change
```javascript
gsap.to('.variable-heading', {
  '--wght': 900,
  scrollTrigger: { trigger: '.variable-heading', start: 'top 80%', end: 'top 20%', scrub: 1 },
});
```

### Best Variable Fonts
| Font | Axes | Source |
|------|------|--------|
| Inter | wght 100-900, wdth 75-100, opsz | Google Fonts |
| Fraunces | wght 100-900, opsz, SOFT, WONK | Google Fonts |
| Roboto Flex | wght, wdth, opsz + 6 more | Google Fonts |

---

## TECHNIQUE 8: Theme-Aware Navigation

Used by: landonorris.com

```html
<nav class="nav" data-theme="dark">...</nav>
<section data-nav-theme="dark" class="hero">...</section>
<section data-nav-theme="light" class="content">...</section>
```

```css
.nav {
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 1000;
  transition: color 0.4s ease;
}

.nav[data-theme="dark"] { color: #fff; }
.nav[data-theme="dark"] a { color: #fff; }
.nav[data-theme="light"] { color: #0f172a; }
.nav[data-theme="light"] a { color: #0f172a; }
```

```javascript
const nav = document.querySelector('.nav');

const observer = new IntersectionObserver(
  (entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        nav.setAttribute('data-theme', entry.target.dataset.navTheme);
      }
    });
  },
  { rootMargin: '-48px 0px -95% 0px' } // 48px = nav height
);

document.querySelectorAll('[data-nav-theme]').forEach((s) => observer.observe(s));
```

---

## TECHNIQUE 9: Image Displacement / Distortion

Used by: kprverse.com, synchronized.studio

### WebGL Hover Distortion (Three.js)
```javascript
import * as THREE from 'three';

const vertexShader = `
  varying vec2 vUv;
  void main() {
    vUv = uv;
    gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
  }
`;

const fragmentShader = `
  uniform sampler2D uTexture;
  uniform sampler2D uDisplacement;
  uniform float uHover;
  uniform vec2 uMouse;
  varying vec2 vUv;

  void main() {
    vec2 uv = vUv;
    float dist = distance(uv, uMouse);
    float strength = smoothstep(0.3, 0.0, dist) * uHover;
    vec4 disp = texture2D(uDisplacement, uv);
    vec2 displaced = uv + disp.rg * strength * 0.1;
    gl_FragColor = texture2D(uTexture, displaced);
  }
`;

// Setup: scene, camera, plane mesh with ShaderMaterial using these shaders
// Track mouse position relative to container, animate uHover 0->1 on enter
```

### CSS-Only Warp (Lightweight)
```css
.image-warp img {
  transition: transform 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.image-warp:hover img {
  transform: perspective(800px) rotateX(5deg) rotateY(-5deg) scale(1.05);
}
```

### SVG Displacement Filter
```html
<svg style="position: absolute; width: 0; height: 0;">
  <filter id="turbulence">
    <feTurbulence type="fractalNoise" baseFrequency="0.015" numOctaves="3" result="noise" />
    <feDisplacementMap in="SourceGraphic" in2="noise" scale="0" />
  </filter>
</svg>
```

```javascript
img.style.filter = 'url(#turbulence)';
img.addEventListener('mouseenter', () => {
  gsap.to('#turbulence feDisplacementMap', { attr: { scale: 30 }, duration: 0.4 });
});
img.addEventListener('mouseleave', () => {
  gsap.to('#turbulence feDisplacementMap', { attr: { scale: 0 }, duration: 0.4 });
});
```

---

## TECHNIQUE 10: Page Transitions

Used by: synchronized.studio, kprverse.com

### Barba.js + GSAP (Production)
```javascript
import barba from '@barba/core';

barba.init({
  transitions: [{
    name: 'clip-transition',
    leave(data) {
      return gsap.to(data.current.container, {
        clipPath: 'inset(0 0 100% 0)',
        duration: 0.6,
        ease: 'power2.inOut',
      });
    },
    enter(data) {
      return gsap.from(data.next.container, {
        clipPath: 'inset(100% 0 0 0)',
        duration: 0.6,
        ease: 'power2.inOut',
      });
    },
    afterEnter() {
      window.scrollTo(0, 0);
      ScrollTrigger.refresh();
    },
  }],
});
```

### Custom Overlay Transition (No Library)
```css
.page-transition {
  position: fixed;
  inset: 0;
  background: #000;
  z-index: 10000;
  clip-path: polygon(0 0, 100% 0, 100% 0%, 0 0%);
  pointer-events: none;
}
```

```javascript
async function navigateTo(url) {
  const overlay = document.querySelector('.page-transition');

  // Cover screen
  await gsap.to(overlay, {
    clipPath: 'polygon(0 0, 100% 0, 100% 100%, 0 100%)',
    duration: 0.5, ease: 'power2.inOut',
  });

  // Fetch + swap content
  const res = await fetch(url);
  const html = await res.text();
  const doc = new DOMParser().parseFromString(html, 'text/html');
  document.querySelector('#main').replaceChildren(
    ...doc.querySelector('#main').childNodes
  );
  window.history.pushState({}, '', url);
  window.scrollTo(0, 0);

  // Reveal
  await gsap.to(overlay, {
    clipPath: 'polygon(0 100%, 100% 100%, 100% 100%, 0 100%)',
    duration: 0.5, ease: 'power2.inOut',
  });
}
```

---

## TECHNIQUE 11: Grayscale to Color Reveals

Used by: stitchingstories.nl

```css
.bw-reveal {
  filter: grayscale(100%);
  transition: filter 0.6s ease;
}

.bw-reveal:hover {
  filter: grayscale(0%);
}
```

### Directional Color Swipe
```css
.color-swipe::after {
  content: '';
  position: absolute;
  inset: 0;
  background: inherit;
  filter: grayscale(100%);
  clip-path: inset(0 0 0 0);
  transition: clip-path 0.8s cubic-bezier(0.77, 0, 0.175, 1);
  pointer-events: none;
}

.color-swipe:hover::after {
  clip-path: inset(0 0 0 100%);
}
```

---

## TECHNIQUE 12: Three.js Post-Processing

Used by: igloo.inc, messenger.abeto.co

### Chromatic Aberration Shader
```glsl
// Fragment shader
uniform sampler2D tDiffuse;
uniform float uOffset;
varying vec2 vUv;

void main() {
  float r = texture2D(tDiffuse, vUv + vec2(uOffset, 0.0)).r;
  float g = texture2D(tDiffuse, vUv).g;
  float b = texture2D(tDiffuse, vUv - vec2(uOffset, 0.0)).b;
  gl_FragColor = vec4(r, g, b, 1.0);
}
```

### CSS Grain Overlay (Lightweight Alternative)
```css
.grain::after {
  content: '';
  position: fixed;
  inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
  pointer-events: none;
  z-index: 9998;
  opacity: 0.4;
  mix-blend-mode: overlay;
}
```

---

## TECHNIQUE 13: Marquee / Infinite Scroll Text

Used by: landonorris.com

```html
<div class="marquee">
  <div class="marquee-track">
    <span>YOUR TEXT HERE —&nbsp;</span>
    <span>YOUR TEXT HERE —&nbsp;</span>
    <span>YOUR TEXT HERE —&nbsp;</span>
    <span>YOUR TEXT HERE —&nbsp;</span>
  </div>
</div>
```

```css
.marquee { overflow: hidden; white-space: nowrap; }

.marquee-track {
  display: inline-flex;
  animation: marquee 20s linear infinite;
}

@keyframes marquee {
  from { transform: translateX(0); }
  to { transform: translateX(-50%); }
}

/* Fade edges */
.marquee {
  mask-image: linear-gradient(90deg, transparent, #000 10%, #000 90%, transparent);
}

/* Speed linked to scroll velocity */
@media (prefers-reduced-motion: reduce) {
  .marquee-track { animation: none; }
}
```

---

## PERFORMANCE GOLDEN RULES

### Only Animate GPU Properties
```
SAFE (composited, no layout/paint):
  - transform (translate, scale, rotate)
  - opacity
  - filter (blur, grayscale, contrast)
  - clip-path
  - background-position (on GPU layers)

AVOID (triggers layout recalculation):
  - width, height, top, left, right, bottom
  - margin, padding, font-size, border-width
```

### will-change Strategy
```css
/* Add BEFORE animation starts, remove AFTER */
.about-to-animate { will-change: transform, opacity; }
.done-animating { will-change: auto; }
/* NEVER apply to more than 10 elements simultaneously */
/* NEVER leave will-change permanently — causes memory bloat */
```

### Device Capability Tiering
```javascript
function getPerformanceTier() {
  const cores = navigator.hardwareConcurrency || 2;
  const memory = navigator.deviceMemory || 4;
  const isMobile = /Mobi|Android/i.test(navigator.userAgent);
  const hasWebGL2 = !!document.createElement('canvas').getContext('webgl2');

  if (isMobile || cores <= 2 || memory <= 2) return 'low';
  if (cores <= 4 || memory <= 4 || !hasWebGL2) return 'medium';
  return 'high';
}

// low = CSS transitions only, no GSAP, no Three.js, no Lenis
// medium = GSAP + Lenis, no Three.js
// high = Full experience: Three.js, post-processing, particles
```

---

*GENESIS Gold Standard — These are the techniques that win Awwwards Site of the Year. Master them all.*
