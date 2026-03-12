# GENESIS Gold Standard — Modern CSS-Native Techniques

Zero-dependency patterns using native browser APIs. These replace JavaScript-heavy approaches
for simpler use cases and progressively enhance the GSAP foundation from TECHNIQUES.md.

**Rule:** Use CSS-native first. Reach for GSAP only when you need timeline sequencing, complex scrub, or cross-browser consistency below 95%.

---

## TECHNIQUE 14: CSS Scroll-Driven Animations

Native CSS replacement for simple GSAP ScrollTrigger patterns. No JavaScript required.

### Browser Support
- Chrome 115+, Edge 115+, Firefox 110+ (behind flag), Safari 18+
- **Fallback:** Use `@supports (animation-timeline: scroll())` to detect, fall back to static state

### Scroll Progress Animation (Replaces `scrub: true`)

```css
/* Progress bar that fills as page scrolls */
.scroll-progress {
  position: fixed;
  top: 0;
  left: 0;
  height: 3px;
  background: var(--accent);
  width: 100%;
  transform-origin: left;
  transform: scaleX(0);
  animation: fill-bar linear;
  animation-timeline: scroll();
  z-index: 9999;
}

@keyframes fill-bar {
  to { transform: scaleX(1); }
}
```

### Element Reveal on Scroll (Replaces ScrollTrigger `start/end`)

```css
.reveal-on-scroll {
  opacity: 0;
  transform: translateY(60px);
  animation: reveal linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 40%;
}

@keyframes reveal {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
```

### Parallax (CSS-Only)

```css
.parallax-image {
  animation: parallax linear;
  animation-timeline: view();
}

@keyframes parallax {
  from { transform: translateY(-15%); }
  to { transform: translateY(15%); }
}
```

### Horizontal Scroll Progress within Section

```css
.horizontal-section {
  animation: slide-left linear;
  animation-timeline: view();
  animation-range: contain 0% contain 100%;
}

@keyframes slide-left {
  from { transform: translateX(0); }
  to { transform: translateX(calc(-100% + 100vw)); }
}
```

### Named Timeline (Pin Equivalent)

```css
.pin-section {
  view-timeline-name: --pin;
  view-timeline-axis: block;
}

.pin-section .content {
  animation: scale-up linear both;
  animation-timeline: --pin;
  animation-range: entry 0% cover 50%;
}

@keyframes scale-up {
  from { transform: scale(0.8); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
}
```

### Feature Detection + GSAP Fallback

```javascript
if (!CSS.supports('animation-timeline', 'scroll()')) {
  // Load GSAP dynamically only when needed
  import('gsap').then(({ gsap }) => {
    import('gsap/ScrollTrigger').then(({ ScrollTrigger }) => {
      gsap.registerPlugin(ScrollTrigger);
      // JS fallback animations here
    });
  });
}
```

### When to Use CSS vs GSAP

| Use Case | CSS Scroll-Driven | GSAP ScrollTrigger |
|----------|-------------------|-------------------|
| Progress bar | Yes | Overkill |
| Simple fade-in on scroll | Yes | Overkill |
| Parallax backgrounds | Yes | Overkill |
| Pinning sections | No (limited) | Yes |
| Timeline sequencing | No | Yes |
| Scrub with easing | No (always linear) | Yes |
| Horizontal scroll with pin | No | Yes |
| Cross-browser < 95% | No | Yes |
| Complex stagger patterns | No | Yes |

---

## TECHNIQUE 15: View Transitions API

Native page transitions replacing Barba.js for SPA and MPA navigation.

### SPA View Transition (Same-Document)

```javascript
// Wrap any DOM update in a view transition
async function navigateTo(url) {
  if (!document.startViewTransition) {
    await swapContent(url);
    return;
  }

  const transition = document.startViewTransition(async () => {
    await swapContent(url);
  });
}

async function swapContent(url) {
  const res = await fetch(url);
  const html = await res.text();
  const doc = new DOMParser().parseFromString(html, 'text/html');

  // NOTE: Using DOMParser + replaceChildren is safe (no script execution).
  // For production, sanitize with DOMPurify if content is user-generated.
  const mainContent = doc.querySelector('#main');
  document.querySelector('#main').replaceChildren(
    ...Array.from(mainContent.childNodes).map(n => document.importNode(n, true))
  );
  document.title = doc.title;
  window.history.pushState({}, '', url);
}
```

### Default Crossfade CSS (Works Automatically)

```css
/* Browser applies these by default — customize to override */
::view-transition-old(root) {
  animation: fade-out 0.3s ease-out;
}

::view-transition-new(root) {
  animation: fade-in 0.3s ease-in;
}
```

### Named View Transitions (Element-Level)

```css
/* Give specific elements unique transition names */
.hero-image {
  view-transition-name: hero;
}

.page-title {
  view-transition-name: title;
}

/* Hero morphs between pages */
::view-transition-old(hero) {
  animation: scale-down 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

::view-transition-new(hero) {
  animation: scale-up 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

@keyframes scale-down {
  to { transform: scale(0.8); opacity: 0; }
}

@keyframes scale-up {
  from { transform: scale(0.8); opacity: 0; }
}
```

### Slide Transition (Page Direction)

```css
/* Slide left when navigating forward */
::view-transition-old(root) {
  animation: slide-out-left 0.35s ease-in-out;
}

::view-transition-new(root) {
  animation: slide-in-right 0.35s ease-in-out;
}

@keyframes slide-out-left {
  to { transform: translateX(-100%); opacity: 0; }
}

@keyframes slide-in-right {
  from { transform: translateX(100%); opacity: 0; }
}
```

### MPA View Transitions (Cross-Document)

```html
<!-- Add to BOTH pages -->
<meta name="view-transition" content="same-origin" />
```

```css
/* Works across full page navigations — no JavaScript needed */
@view-transition {
  navigation: auto;
}

/* Shared elements morph automatically if view-transition-name matches */
.product-card {
  view-transition-name: product;
}
```

### Next.js Integration

```jsx
'use client';
import { useRouter } from 'next/navigation';

function TransitionLink({ href, children }) {
  const router = useRouter();

  function handleClick(e) {
    e.preventDefault();
    if (document.startViewTransition) {
      document.startViewTransition(() => router.push(href));
    } else {
      router.push(href);
    }
  }

  return <a href={href} onClick={handleClick}>{children}</a>;
}
```

### When to Use View Transitions vs Barba.js

| Feature | View Transitions API | Barba.js |
|---------|---------------------|----------|
| Browser support | 90%+ (2026) | 100% |
| Element morphing | Native `view-transition-name` | Manual GSAP |
| MPA support | Yes (meta tag) | Yes |
| Complex sequences | Limited | Full GSAP timeline |
| Bundle size | 0 KB | ~15 KB |
| Back/forward cache | Native | Manual |

---

## TECHNIQUE 16: Container Queries

Component-level responsive design. Elements respond to their container size, not viewport.

### Setup

```css
/* Parent must declare containment */
.card-grid {
  container-type: inline-size;
  container-name: cards;
}

/* Children respond to container width */
.card {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

@container cards (min-width: 500px) {
  .card {
    grid-template-columns: 200px 1fr;
  }
}

@container cards (min-width: 800px) {
  .card {
    grid-template-columns: 300px 1fr auto;
  }
}
```

### Sidebar-Aware Components

```css
/* Component adapts whether sidebar is open or closed */
.content-area {
  container-type: inline-size;
  container-name: content;
}

.dashboard-widget {
  padding: 1rem;
}

@container content (min-width: 600px) {
  .dashboard-widget {
    padding: 2rem;
    font-size: 1.125rem;
  }
}

@container content (min-width: 1000px) {
  .dashboard-widget {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
  }
}
```

### Container Query Units

```css
/* cqw = 1% of container width, cqh = 1% of container height */
.responsive-text {
  font-size: clamp(1rem, 4cqw, 2.5rem);
}

.fluid-padding {
  padding: clamp(0.5rem, 3cqw, 3rem);
}
```

---

## TECHNIQUE 17: :has() Relational Selector

Parent styling based on child state. The "parent selector" CSS never had until now.

### Form Validation Styling

```css
/* Label turns green when its input is valid */
.form-group:has(input:valid) label {
  color: #22c55e;
}

.form-group:has(input:invalid:not(:placeholder-shown)) label {
  color: #ef4444;
}

/* Form group with error message shown */
.form-group:has(.error:not(:empty)) {
  border-left: 3px solid #ef4444;
  padding-left: 1rem;
}
```

### Card with Image vs Without

```css
/* Cards with images get different layout */
.card:has(img) {
  grid-template-rows: 200px 1fr;
}

.card:not(:has(img)) {
  grid-template-rows: 1fr;
  padding-top: 2rem;
}
```

### Interactive State Propagation

```css
/* Highlight entire row when checkbox is checked */
tr:has(input[type="checkbox"]:checked) {
  background: rgba(99, 102, 241, 0.1);
}

/* Navbar changes when page has a hero (dark bg) */
body:has(.hero-dark) .navbar {
  color: white;
  --nav-bg: transparent;
}

/* Sidebar collapse affects main content */
body:has(.sidebar.collapsed) .main-content {
  margin-left: 4rem;
}

body:has(.sidebar:not(.collapsed)) .main-content {
  margin-left: 16rem;
}
```

### Empty State Detection

```css
/* Show empty state when list has no items */
.list:has(> :first-child) .empty-state {
  display: none;
}

.list:not(:has(> :first-child)) .empty-state {
  display: flex;
}
```

---

## TECHNIQUE 18: Popover API + Anchor Positioning

Native modals, tooltips, and dropdowns — no JavaScript positioning libraries needed.

### Basic Popover

```html
<button popovertarget="menu">Open Menu</button>

<div id="menu" popover>
  <nav>
    <a href="/about">About</a>
    <a href="/work">Work</a>
    <a href="/contact">Contact</a>
  </nav>
</div>
```

```css
/* Popover comes with built-in backdrop and top-layer positioning */
[popover] {
  border: none;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
  max-width: 320px;
  background: var(--surface);

  /* Entry/exit animations */
  opacity: 0;
  transform: translateY(8px) scale(0.96);
  transition: opacity 0.2s, transform 0.2s, display 0.2s allow-discrete;
}

[popover]:popover-open {
  opacity: 1;
  transform: translateY(0) scale(1);
}

/* Starting style for entry animation */
@starting-style {
  [popover]:popover-open {
    opacity: 0;
    transform: translateY(8px) scale(0.96);
  }
}

/* Backdrop */
[popover]::backdrop {
  background: rgba(0, 0, 0, 0);
  transition: background 0.3s;
}

[popover]:popover-open::backdrop {
  background: rgba(0, 0, 0, 0.3);
  backdrop-filter: blur(4px);
}
```

### CSS Anchor Positioning (Tooltips)

```css
.trigger {
  anchor-name: --trigger;
}

.tooltip {
  position: fixed;
  position-anchor: --trigger;
  top: anchor(--trigger bottom);
  left: anchor(--trigger center);
  translate: -50% 8px;

  /* Flip if near edge */
  position-try-fallbacks: flip-block;
}
```

### Modal Pattern (No JS Positioning)

```html
<button popovertarget="modal">Open Modal</button>

<div id="modal" popover="manual">
  <h2>Confirm Action</h2>
  <p>This cannot be undone.</p>
  <div class="modal-actions">
    <button popovertarget="modal" popovertargetaction="hide">Cancel</button>
    <button class="confirm-btn">Confirm</button>
  </div>
</div>
```

```css
/* popover="manual" = must be dismissed explicitly (no light dismiss) */
#modal {
  margin: auto; /* Centers in viewport */
  max-width: 480px;
  width: 90vw;
  border-radius: 16px;
  padding: 2rem;
}
```

---

## TECHNIQUE 19: Rive Animations

Next-gen interactive animations. 10x smaller than Lottie, with state machines and runtime interactivity.

### Why Rive > Lottie
| Feature | Rive | Lottie |
|---------|------|--------|
| File size | 5-50 KB typical | 50-500 KB |
| Interactivity | State machines, inputs | Play/pause only |
| Runtime control | Full API | Limited |
| Design tool | Rive.app (free) | After Effects ($) |
| Performance | Custom renderer (WASM) | SVG/Canvas |

### Vanilla JS

```html
<canvas id="rive-canvas" width="500" height="500"></canvas>
<script src="https://unpkg.com/@rive-app/canvas@latest"></script>

<script>
  const riveInstance = new rive.Rive({
    src: '/animations/hero.riv',
    canvas: document.getElementById('rive-canvas'),
    autoplay: true,
    stateMachines: 'MainState',
    onLoad: () => {
      const inputs = riveInstance.stateMachineInputs('MainState');
      const hoverInput = inputs.find(i => i.name === 'isHovering');

      document.getElementById('rive-canvas').addEventListener('mouseenter', () => {
        hoverInput.value = true;
      });
      document.getElementById('rive-canvas').addEventListener('mouseleave', () => {
        hoverInput.value = false;
      });
    },
  });
</script>
```

### React Component

```jsx
import { useRive, useStateMachineInput } from '@rive-app/react-canvas';
import { useEffect } from 'react';

function AnimatedIcon({ isActive }) {
  const { rive, RiveComponent } = useRive({
    src: '/animations/icon.riv',
    stateMachines: 'MainState',
    autoplay: true,
  });

  const activeInput = useStateMachineInput(rive, 'MainState', 'isActive');

  useEffect(() => {
    if (activeInput) activeInput.value = isActive;
  }, [isActive, activeInput]);

  return <RiveComponent style={{ width: 48, height: 48 }} />;
}
```

### Next.js (Dynamic Import)

```jsx
import dynamic from 'next/dynamic';

const RiveAnimation = dynamic(
  () => import('@rive-app/react-canvas').then(mod => {
    const { useRive } = mod;
    return function RiveWrapper({ src, stateMachine }) {
      const { RiveComponent } = useRive({
        src,
        stateMachines: stateMachine,
        autoplay: true,
      });
      return <RiveComponent style={{ width: '100%', height: '100%' }} />;
    };
  }),
  { ssr: false }
);
```

### Use Cases on Award-Winning Sites
- **Interactive icons** — hamburger menu, like button, loading states
- **Onboarding flows** — animated illustrations that respond to scroll/input
- **Hero animations** — complex character/scene animations at tiny file sizes
- **Micro-feedback** — success/error/warning animated indicators
- **Cursor followers** — animated elements that track cursor with state changes

### Performance
- `.riv` files are typically 5-50 KB (vs 200KB+ Lottie)
- WASM renderer = near-native performance
- Use `shouldDisableRiveListeners={true}` if not using click/hover inputs
- Lazy load with `IntersectionObserver` for below-fold animations

---

## TECHNIQUE 20: Advanced GSAP Plugins

Beyond ScrollTrigger — the plugins that make $50K agency work possible.

### GSAP Flip (Layout Animations)

Animate between layout states without calculating positions manually.

```javascript
import { Flip } from 'gsap/Flip';
gsap.registerPlugin(Flip);

// 1. Capture current state
const state = Flip.getState('.cards .card');

// 2. Make DOM changes (reorder, reparent, toggle classes)
document.querySelector('.cards').classList.toggle('grid-view');

// 3. Animate from old positions to new
Flip.from(state, {
  duration: 0.6,
  ease: 'power2.inOut',
  stagger: 0.05,
  absolute: true, // Prevents layout thrashing during animation
  onEnter: elements => gsap.fromTo(elements,
    { opacity: 0, scale: 0 },
    { opacity: 1, scale: 1, duration: 0.4 }
  ),
  onLeave: elements => gsap.to(elements,
    { opacity: 0, scale: 0, duration: 0.4 }
  ),
});
```

**Use for:** Grid/list toggle, filtering/sorting, tab content swap, drag-and-drop reorder, shared element transitions.

### GSAP MotionPath

Move elements along SVG or custom paths.

```javascript
import { MotionPathPlugin } from 'gsap/MotionPathPlugin';
gsap.registerPlugin(MotionPathPlugin);

// Follow an SVG path
gsap.to('.rocket', {
  motionPath: {
    path: '#flight-path',
    align: '#flight-path',
    autoRotate: true,
    alignOrigin: [0.5, 0.5],
  },
  duration: 3,
  ease: 'power1.inOut',
  scrollTrigger: {
    trigger: '.journey-section',
    start: 'top top',
    end: 'bottom bottom',
    scrub: 1,
  },
});

// Custom path (array of points)
gsap.to('.dot', {
  motionPath: {
    path: [
      { x: 0, y: 0 },
      { x: 200, y: -100 },
      { x: 400, y: 50 },
      { x: 600, y: -200 },
    ],
    curviness: 1.5,
  },
  duration: 2,
});
```

**Use for:** Scroll storytelling, path-following icons, animated diagrams, onboarding flows.

### GSAP DrawSVG

Animate SVG stroke drawing.

```javascript
import { DrawSVGPlugin } from 'gsap/DrawSVGPlugin';
gsap.registerPlugin(DrawSVGPlugin);

// Draw from nothing to full
gsap.from('.svg-line', {
  drawSVG: '0%',
  duration: 1.5,
  ease: 'power2.inOut',
  scrollTrigger: { trigger: '.svg-section', start: 'top 70%' },
});

// Draw from center outward
gsap.from('.svg-circle', { drawSVG: '50% 50%', duration: 1 });

// Animated signature
gsap.from('.signature path', {
  drawSVG: '0%',
  duration: 2,
  stagger: 0.3,
  ease: 'none',
});
```

**Use for:** Logo reveals, animated illustrations, process diagrams, signature animations, connection lines.

### GSAP MorphSVG

Morph one SVG shape into another.

```javascript
import { MorphSVGPlugin } from 'gsap/MorphSVGPlugin';
gsap.registerPlugin(MorphSVGPlugin);

// Morph circle to star
gsap.to('#circle', {
  morphSVG: '#star',
  duration: 1,
  ease: 'elastic.out(1, 0.5)',
});

// Morph with rotation
gsap.to('#shape1', {
  morphSVG: { shape: '#shape2', type: 'rotational', origin: '50% 50%' },
  duration: 1.2,
});
```

**Use for:** Icon morphing (hamburger to X, play to pause), shape transitions, loading indicators, decorative elements.

### GSAP ScrollSmoother (Premium Lenis Alternative)

```javascript
import { ScrollSmoother } from 'gsap/ScrollSmoother';
gsap.registerPlugin(ScrollSmoother, ScrollTrigger);

// Requires wrapper structure:
// <div id="smooth-wrapper"><div id="smooth-content">...</div></div>

const smoother = ScrollSmoother.create({
  wrapper: '#smooth-wrapper',
  content: '#smooth-content',
  smooth: 1.5,
  effects: true, // Enable data-speed and data-lag attributes
  smoothTouch: false,
});
```

```html
<!-- Parallax via data attributes -->
<img data-speed="0.5" src="bg.jpg" />  <!-- Moves at half scroll speed -->
<h1 data-speed="1.2">Title</h1>       <!-- Moves 20% faster -->
<div data-lag="0.8">Delayed</div>      <!-- Follows with lag -->
```

**Use for:** Full-page smooth scroll when already using GSAP Club membership (avoids Lenis dependency).

---

## PERFORMANCE: Modern CSS vs JavaScript Decision Matrix

```
STEP 1: Can pure CSS do it?
  Yes -> Use CSS (scroll-driven, view-transition, :has, container query)
  Partially -> CSS + minimal JS enhancement
  No -> GSAP / Three.js

STEP 2: Does it need sequencing or complex control?
  Yes -> GSAP timeline
  No -> CSS animation/transition

STEP 3: Does it need 3D or shader effects?
  Yes -> Three.js / WebGL
  No -> Stay 2D

STEP 4: Does it need interactive state machines?
  Yes -> Rive
  No -> CSS or GSAP
```

---

*GENESIS Gold Standard — Modern CSS-native patterns eliminate dependencies. Use the platform first.*
