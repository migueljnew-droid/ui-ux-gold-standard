# GENESIS Gold Standard — Micro-Interactions & Component Choreography

The 1% details that separate $50K agency work from templates.
Every component here includes production-ready code with exact timing functions.

**Rule:** Micro-interactions must feel inevitable, not decorative. If removing the animation
makes the interface worse to *use* (not just worse to look at), it belongs.

---

## TECHNIQUE 21: Button Micro-Interactions

### Fill Sweep on Hover

```css
.btn-sweep {
  position: relative;
  overflow: hidden;
  background: transparent;
  color: #fff;
  border: 2px solid #fff;
  padding: 0.875rem 2rem;
  cursor: pointer;
  z-index: 1;
  transition: color 0.4s ease;
}

.btn-sweep::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #fff;
  transform: translateX(-101%);
  transition: transform 0.4s cubic-bezier(0.65, 0, 0.35, 1);
  z-index: -1;
}

.btn-sweep:hover {
  color: #000;
}

.btn-sweep:hover::before {
  transform: translateX(0);
}
```

### Scale + Shadow Lift on Press

```css
.btn-lift {
  background: var(--accent);
  color: #fff;
  padding: 0.875rem 2rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: transform 0.2s cubic-bezier(0.34, 1.56, 0.64, 1),
              box-shadow 0.2s ease;
}

.btn-lift:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.2);
}

.btn-lift:active {
  transform: translateY(0) scale(0.97);
  box-shadow: 0 2px 8px -2px rgba(0, 0, 0, 0.15);
  transition-duration: 0.1s;
}
```

### Submit Button State Machine (Loading to Success)

```html
<button class="btn-submit" data-state="idle">
  <span class="btn-text">Submit</span>
  <span class="btn-loader"></span>
  <span class="btn-check">
    <svg viewBox="0 0 24 24" width="20" height="20">
      <path d="M5 13l4 4L19 7" fill="none" stroke="currentColor"
            stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
    </svg>
  </span>
</button>
```

```css
.btn-submit {
  position: relative;
  min-width: 140px;
  padding: 0.875rem 2rem;
  background: var(--accent);
  color: #fff;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  overflow: hidden;
  transition: min-width 0.3s cubic-bezier(0.4, 0, 0.2, 1),
              background 0.3s ease,
              border-radius 0.3s ease;
}

.btn-text, .btn-loader, .btn-check {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  transition: opacity 0.2s, transform 0.2s;
}

.btn-loader, .btn-check { opacity: 0; transform: translate(-50%, -50%) scale(0); }
.btn-submit[data-state="idle"] .btn-text { opacity: 1; }

/* Loading state */
.btn-submit[data-state="loading"] {
  min-width: 52px;
  border-radius: 26px;
  pointer-events: none;
}
.btn-submit[data-state="loading"] .btn-text { opacity: 0; }
.btn-submit[data-state="loading"] .btn-loader {
  opacity: 1;
  transform: translate(-50%, -50%) scale(1);
}

.btn-loader {
  width: 20px;
  height: 20px;
  border: 2px solid rgba(255,255,255,0.3);
  border-top-color: #fff;
  border-radius: 50%;
  animation: spin 0.6s linear infinite;
}

@keyframes spin { to { transform: translate(-50%, -50%) rotate(360deg); } }

/* Success state */
.btn-submit[data-state="success"] {
  min-width: 52px;
  border-radius: 26px;
  background: #22c55e;
  pointer-events: none;
}
.btn-submit[data-state="success"] .btn-text { opacity: 0; }
.btn-submit[data-state="success"] .btn-loader { opacity: 0; }
.btn-submit[data-state="success"] .btn-check {
  opacity: 1;
  transform: translate(-50%, -50%) scale(1);
}

.btn-check svg path {
  stroke-dasharray: 24;
  stroke-dashoffset: 24;
}
.btn-submit[data-state="success"] .btn-check svg path {
  animation: draw-check 0.4s 0.1s ease forwards;
}

@keyframes draw-check {
  to { stroke-dashoffset: 0; }
}
```

```javascript
async function handleSubmit(btn) {
  btn.dataset.state = 'loading';
  try {
    await submitForm(); // your async operation
    btn.dataset.state = 'success';
    setTimeout(() => { btn.dataset.state = 'idle'; }, 2000);
  } catch {
    btn.dataset.state = 'idle';
    btn.classList.add('shake');
    btn.addEventListener('animationend', () => btn.classList.remove('shake'), { once: true });
  }
}
```

---

## TECHNIQUE 22: Input & Form Animations

### Floating Label

```html
<div class="input-group">
  <input type="text" id="name" placeholder=" " required />
  <label for="name">Full Name</label>
  <div class="input-line"></div>
</div>
```

```css
.input-group {
  position: relative;
  margin-bottom: 1.5rem;
}

.input-group input {
  width: 100%;
  padding: 1.25rem 0 0.5rem;
  border: none;
  border-bottom: 2px solid #333;
  background: transparent;
  font-size: 1rem;
  color: #fff;
  outline: none;
  transition: border-color 0.3s;
}

.input-group label {
  position: absolute;
  top: 1rem;
  left: 0;
  font-size: 1rem;
  color: #666;
  pointer-events: none;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

/* Float up when focused OR has content (placeholder-shown trick) */
.input-group input:focus + label,
.input-group input:not(:placeholder-shown) + label {
  top: 0;
  font-size: 0.75rem;
  color: var(--accent);
}

/* Underline draws from center */
.input-line {
  position: absolute;
  bottom: 0;
  left: 50%;
  width: 0;
  height: 2px;
  background: var(--accent);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.input-group input:focus ~ .input-line {
  left: 0;
  width: 100%;
}
```

### Shake on Validation Error

```css
@keyframes shake {
  0%, 100% { transform: translateX(0); }
  10%, 30%, 50%, 70%, 90% { transform: translateX(-4px); }
  20%, 40%, 60%, 80% { transform: translateX(4px); }
}

.input-group.error input {
  border-color: #ef4444;
  animation: shake 0.5s cubic-bezier(0.36, 0.07, 0.19, 0.97);
}

.input-group.error label {
  color: #ef4444;
}
```

### Character Counter with Pulse

```css
.char-counter {
  font-size: 0.75rem;
  color: #666;
  text-align: right;
  transition: color 0.2s;
}

.char-counter.warning {
  color: #f59e0b;
  animation: pulse-subtle 1s ease infinite;
}

.char-counter.danger {
  color: #ef4444;
  font-weight: 600;
}

@keyframes pulse-subtle {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.6; }
}
```

---

## TECHNIQUE 23: Card Hover Effects

### 3D Tilt Following Cursor

```javascript
document.querySelectorAll('.tilt-card').forEach(card => {
  card.addEventListener('mousemove', (e) => {
    const rect = card.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;
    const centerX = rect.width / 2;
    const centerY = rect.height / 2;

    const rotateX = ((y - centerY) / centerY) * -8; // max 8deg
    const rotateY = ((x - centerX) / centerX) * 8;

    card.style.transform =
      `perspective(800px) rotateX(${rotateX}deg) rotateY(${rotateY}deg) scale(1.02)`;
  });

  card.addEventListener('mouseleave', () => {
    card.style.transform = 'perspective(800px) rotateX(0) rotateY(0) scale(1)';
  });
});
```

```css
.tilt-card {
  transition: transform 0.15s ease-out, box-shadow 0.3s ease;
  will-change: transform;
  transform-style: preserve-3d;
}

.tilt-card:hover {
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
}
```

### Spotlight / Glare Effect

```css
.spotlight-card {
  position: relative;
  overflow: hidden;
  background: #111;
  border-radius: 12px;
}

.spotlight-card::before {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(
    400px circle at var(--mouse-x) var(--mouse-y),
    rgba(255, 255, 255, 0.06),
    transparent 40%
  );
  pointer-events: none;
  opacity: 0;
  transition: opacity 0.3s;
}

.spotlight-card:hover::before {
  opacity: 1;
}
```

```javascript
document.querySelectorAll('.spotlight-card').forEach(card => {
  card.addEventListener('mousemove', (e) => {
    const rect = card.getBoundingClientRect();
    card.style.setProperty('--mouse-x', (e.clientX - rect.left) + 'px');
    card.style.setProperty('--mouse-y', (e.clientY - rect.top) + 'px');
  });
});
```

### Animated Border Glow

```css
.glow-card {
  position: relative;
  background: #0a0a0a;
  border-radius: 12px;
  padding: 2rem;
}

.glow-card::before {
  content: '';
  position: absolute;
  inset: -1px;
  border-radius: inherit;
  background: conic-gradient(
    from var(--angle, 0deg),
    transparent 30%,
    var(--accent) 50%,
    transparent 70%
  );
  z-index: -1;
  opacity: 0;
  transition: opacity 0.4s;
}

.glow-card:hover::before {
  opacity: 1;
  animation: rotate-glow 3s linear infinite;
}

@property --angle {
  syntax: '<angle>';
  initial-value: 0deg;
  inherits: false;
}

@keyframes rotate-glow {
  to { --angle: 360deg; }
}
```

---

## TECHNIQUE 24: Navigation Choreography

### Navbar Transform on Scroll

```css
.nav {
  position: fixed;
  top: 0;
  width: 100%;
  padding: 1.5rem 2rem;
  z-index: 1000;
  transition: padding 0.3s ease,
              background 0.3s ease,
              backdrop-filter 0.3s ease;
}

.nav.scrolled {
  padding: 0.75rem 2rem;
  background: rgba(0, 0, 0, 0.8);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
}
```

```javascript
let lastScroll = 0;
const nav = document.querySelector('.nav');

window.addEventListener('scroll', () => {
  const scrollY = window.scrollY;
  nav.classList.toggle('scrolled', scrollY > 80);

  // Auto-hide on scroll down, show on scroll up
  if (scrollY > lastScroll && scrollY > 200) {
    nav.style.transform = 'translateY(-100%)';
  } else {
    nav.style.transform = 'translateY(0)';
  }
  lastScroll = scrollY;
}, { passive: true });
```

### Full-Screen Menu Reveal

```css
.menu-overlay {
  position: fixed;
  inset: 0;
  background: #000;
  z-index: 999;
  clip-path: circle(0% at calc(100% - 2rem) 2rem);
  transition: clip-path 0.6s cubic-bezier(0.65, 0, 0.35, 1);
  display: flex;
  align-items: center;
  justify-content: center;
}

.menu-overlay.open {
  clip-path: circle(150% at calc(100% - 2rem) 2rem);
}

.menu-overlay nav a {
  display: block;
  font-size: clamp(2rem, 6vw, 4rem);
  font-weight: 700;
  color: #fff;
  text-decoration: none;
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.4s ease, transform 0.4s ease;
}

.menu-overlay.open nav a {
  opacity: 1;
  transform: translateY(0);
}

/* Stagger with transition-delay */
.menu-overlay.open nav a:nth-child(1) { transition-delay: 0.2s; }
.menu-overlay.open nav a:nth-child(2) { transition-delay: 0.3s; }
.menu-overlay.open nav a:nth-child(3) { transition-delay: 0.4s; }
.menu-overlay.open nav a:nth-child(4) { transition-delay: 0.5s; }
.menu-overlay.open nav a:nth-child(5) { transition-delay: 0.6s; }
```

### Sliding Active Link Underline

```css
.nav-links {
  display: flex;
  gap: 2rem;
  position: relative;
}

.nav-indicator {
  position: absolute;
  bottom: 0;
  height: 2px;
  background: var(--accent);
  transition: left 0.3s cubic-bezier(0.4, 0, 0.2, 1),
              width 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}
```

```javascript
const links = document.querySelectorAll('.nav-links a');
const indicator = document.querySelector('.nav-indicator');

function updateIndicator(el) {
  indicator.style.left = el.offsetLeft + 'px';
  indicator.style.width = el.offsetWidth + 'px';
}

links.forEach(link => {
  link.addEventListener('mouseenter', () => updateIndicator(link));
});

// Set initial position on active link
const active = document.querySelector('.nav-links a.active');
if (active) updateIndicator(active);
```

### Hamburger to X Morphing (CSS Only)

```html
<button class="hamburger" aria-label="Toggle menu" aria-expanded="false">
  <span></span>
  <span></span>
  <span></span>
</button>
```

```css
.hamburger {
  display: flex;
  flex-direction: column;
  gap: 6px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
}

.hamburger span {
  display: block;
  width: 24px;
  height: 2px;
  background: #fff;
  transition: transform 0.3s cubic-bezier(0.65, 0, 0.35, 1),
              opacity 0.2s ease;
  transform-origin: center;
}

.hamburger.active span:nth-child(1) {
  transform: translateY(8px) rotate(45deg);
}

.hamburger.active span:nth-child(2) {
  opacity: 0;
  transform: scaleX(0);
}

.hamburger.active span:nth-child(3) {
  transform: translateY(-8px) rotate(-45deg);
}
```

---

## TECHNIQUE 25: Toast & Notification Animations

### Slide-In with Auto-Dismiss Progress

```html
<div class="toast-container" aria-live="polite"></div>
```

```css
.toast-container {
  position: fixed;
  bottom: 1.5rem;
  right: 1.5rem;
  display: flex;
  flex-direction: column-reverse;
  gap: 0.75rem;
  z-index: 10000;
}

.toast {
  min-width: 300px;
  padding: 1rem 1.25rem;
  background: #1a1a1a;
  color: #fff;
  border-radius: 10px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
  transform: translateX(120%);
  animation: toast-in 0.4s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
  position: relative;
  overflow: hidden;
}

.toast.removing {
  animation: toast-out 0.3s ease-in forwards;
}

@keyframes toast-in {
  to { transform: translateX(0); }
}

@keyframes toast-out {
  to { transform: translateX(120%); opacity: 0; }
}

/* Auto-dismiss progress bar */
.toast::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  height: 3px;
  background: var(--accent);
  animation: toast-timer var(--duration, 4s) linear forwards;
}

@keyframes toast-timer {
  from { width: 100%; }
  to { width: 0%; }
}
```

```javascript
function showToast(message, { duration = 4000, type = 'info' } = {}) {
  const container = document.querySelector('.toast-container');
  const toast = document.createElement('div');
  toast.className = `toast toast-${type}`;
  toast.style.setProperty('--duration', duration + 'ms');
  toast.textContent = message;
  toast.setAttribute('role', 'status');

  container.appendChild(toast);

  const timer = setTimeout(() => dismissToast(toast), duration);

  // Pause on hover
  toast.addEventListener('mouseenter', () => {
    clearTimeout(timer);
    toast.style.animationPlayState = 'paused';
    toast.querySelector('::after')?.style.setProperty('animation-play-state', 'paused');
  });

  return toast;
}

function dismissToast(toast) {
  toast.classList.add('removing');
  toast.addEventListener('animationend', () => toast.remove());
}
```

---

## TECHNIQUE 26: Modal & Drawer Choreography

### Scale-From-Origin Modal

```css
.modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0);
  backdrop-filter: blur(0px);
  z-index: 9999;
  display: grid;
  place-items: center;
  opacity: 0;
  pointer-events: none;
  transition: background 0.3s, backdrop-filter 0.3s, opacity 0.01s 0.3s;
}

.modal-backdrop.open {
  opacity: 1;
  pointer-events: auto;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(8px);
  transition: background 0.3s, backdrop-filter 0.3s;
}

.modal-content {
  background: #1a1a1a;
  border-radius: 16px;
  padding: 2rem;
  max-width: 500px;
  width: 90vw;
  transform: scale(0.9);
  opacity: 0;
  transition: transform 0.35s cubic-bezier(0.34, 1.56, 0.64, 1),
              opacity 0.25s ease;
}

.modal-backdrop.open .modal-content {
  transform: scale(1);
  opacity: 1;
  transition-delay: 0.1s;
}
```

### Drawer with Spring Physics

```css
.drawer {
  position: fixed;
  top: 0;
  right: 0;
  width: min(400px, 90vw);
  height: 100vh;
  background: #111;
  z-index: 10000;
  transform: translateX(100%);
  transition: transform 0.5s cubic-bezier(0.32, 0.72, 0, 1); /* Spring-like */
  overflow-y: auto;
  overscroll-behavior: contain;
}

.drawer.open {
  transform: translateX(0);
}

/* Content push effect */
.page-content {
  transition: transform 0.5s cubic-bezier(0.32, 0.72, 0, 1),
              filter 0.5s ease;
}

body.drawer-open .page-content {
  transform: translateX(-50px) scale(0.97);
  filter: brightness(0.7);
}
```

---

## TECHNIQUE 27: Loading & Skeleton Patterns

### Shimmer Gradient Sweep

```css
.skeleton {
  background: #1a1a1a;
  border-radius: 8px;
  position: relative;
  overflow: hidden;
}

.skeleton::after {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    90deg,
    transparent 0%,
    rgba(255, 255, 255, 0.04) 50%,
    transparent 100%
  );
  transform: translateX(-100%);
  animation: shimmer 1.5s ease-in-out infinite;
}

@keyframes shimmer {
  to { transform: translateX(100%); }
}

/* Skeleton shapes */
.skeleton-text { height: 1rem; margin-bottom: 0.75rem; }
.skeleton-text:last-child { width: 60%; }
.skeleton-heading { height: 1.5rem; width: 40%; margin-bottom: 1rem; }
.skeleton-avatar { width: 48px; height: 48px; border-radius: 50%; }
.skeleton-image { width: 100%; aspect-ratio: 16/9; }
```

### Progressive Image Loading (Blur-Up)

```html
<div class="progressive-image">
  <img
    src="placeholder-20px.jpg"
    data-src="full-image.jpg"
    alt="Description"
    class="blur-up"
  />
</div>
```

```css
.progressive-image {
  position: relative;
  overflow: hidden;
  background: #111;
}

.blur-up {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: blur(20px);
  transform: scale(1.1); /* Hide blur edges */
  transition: filter 0.6s ease, transform 0.6s ease;
}

.blur-up.loaded {
  filter: blur(0);
  transform: scale(1);
}
```

```javascript
// IntersectionObserver for lazy loading
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      const fullSrc = img.dataset.src;

      // Preload full image
      const preloader = new Image();
      preloader.onload = () => {
        img.src = fullSrc;
        img.classList.add('loaded');
      };
      preloader.src = fullSrc;

      observer.unobserve(img);
    }
  });
}, { rootMargin: '200px' });

document.querySelectorAll('.blur-up').forEach(img => observer.observe(img));
```

### Content Crossfade (Skeleton to Real)

```css
.content-placeholder {
  position: relative;
}

.content-placeholder .skeleton-layer {
  transition: opacity 0.4s ease;
}

.content-placeholder .real-content {
  opacity: 0;
  transition: opacity 0.4s ease 0.1s;
}

.content-placeholder.loaded .skeleton-layer {
  opacity: 0;
  pointer-events: none;
}

.content-placeholder.loaded .real-content {
  opacity: 1;
}
```

---

## TECHNIQUE 28: WebGL Shader Gallery

Premium visual effects beyond basic displacement.

### Noise Distortion (Hover Effect)

```glsl
// Fragment shader — applies animated noise distortion
uniform sampler2D uTexture;
uniform float uTime;
uniform float uIntensity;
varying vec2 vUv;

// Simplex noise function (2D)
vec3 permute(vec3 x) { return mod(((x*34.0)+1.0)*x, 289.0); }

float snoise(vec2 v) {
  const vec4 C = vec4(0.211324865405187, 0.366025403784439,
                      -0.577350269189626, 0.024390243902439);
  vec2 i = floor(v + dot(v, C.yy));
  vec2 x0 = v - i + dot(i, C.xx);
  vec2 i1 = (x0.x > x0.y) ? vec2(1.0, 0.0) : vec2(0.0, 1.0);
  vec4 x12 = x0.xyxy + C.xxzz;
  x12.xy -= i1;
  i = mod(i, 289.0);
  vec3 p = permute(permute(i.y + vec3(0.0, i1.y, 1.0)) + i.x + vec3(0.0, i1.x, 1.0));
  vec3 m = max(0.5 - vec3(dot(x0,x0), dot(x12.xy,x12.xy), dot(x12.zw,x12.zw)), 0.0);
  m = m*m; m = m*m;
  vec3 x = 2.0 * fract(p * C.www) - 1.0;
  vec3 h = abs(x) - 0.5;
  vec3 a0 = x - floor(x + 0.5);
  m *= 1.79284291400159 - 0.85373472095314 * (a0*a0 + h*h);
  vec3 g;
  g.x = a0.x * x0.x + h.x * x0.y;
  g.yz = a0.yz * x12.xz + h.yz * x12.yw;
  return 130.0 * dot(m, g);
}

void main() {
  vec2 uv = vUv;
  float noise = snoise(uv * 3.0 + uTime * 0.5);
  uv += noise * uIntensity * 0.02;
  gl_FragColor = texture2D(uTexture, uv);
}
```

### Gradient Mesh (Animated Background)

```css
/* CSS-only animated gradient mesh — no WebGL needed */
.gradient-mesh {
  background:
    radial-gradient(at 40% 20%, #7c3aed 0px, transparent 50%),
    radial-gradient(at 80% 0%, #06b6d4 0px, transparent 50%),
    radial-gradient(at 0% 50%, #f43f5e 0px, transparent 50%),
    radial-gradient(at 80% 50%, #eab308 0px, transparent 50%),
    radial-gradient(at 0% 100%, #6366f1 0px, transparent 50%),
    radial-gradient(at 80% 100%, #22c55e 0px, transparent 50%);
  background-color: #0a0a0a;
  animation: mesh-shift 15s ease infinite alternate;
}

@keyframes mesh-shift {
  0% { background-position: 0% 0%, 100% 0%, 0% 100%, 100% 50%, 0% 100%, 100% 100%; }
  100% { background-position: 100% 100%, 0% 100%, 100% 0%, 0% 50%, 100% 0%, 0% 0%; }
}
```

### Metaball Effect (CSS)

```css
/* Combines blur + contrast trick from Technique 6 with animated positions */
.metaballs {
  filter: blur(25px) contrast(30);
  background: #000;
  position: relative;
  height: 400px;
  overflow: hidden;
}

.metaball {
  position: absolute;
  width: 150px;
  height: 150px;
  border-radius: 50%;
  background: var(--accent);
}

.metaball:nth-child(1) { animation: float-1 8s ease-in-out infinite; }
.metaball:nth-child(2) { animation: float-2 10s ease-in-out infinite; }
.metaball:nth-child(3) { animation: float-3 12s ease-in-out infinite; }

@keyframes float-1 {
  0%, 100% { transform: translate(20%, 20%); }
  33% { transform: translate(60%, 50%); }
  66% { transform: translate(30%, 70%); }
}

@keyframes float-2 {
  0%, 100% { transform: translate(70%, 30%); }
  33% { transform: translate(20%, 60%); }
  66% { transform: translate(80%, 20%); }
}

@keyframes float-3 {
  0%, 100% { transform: translate(50%, 80%); }
  33% { transform: translate(80%, 40%); }
  66% { transform: translate(10%, 30%); }
}
```

---

## PREMIUM TIMING FUNCTIONS

The exact easing curves that make interactions feel expensive:

```css
:root {
  /* Standard motion */
  --ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-out-quint: cubic-bezier(0.22, 1, 0.36, 1);
  --ease-in-out-quint: cubic-bezier(0.83, 0, 0.17, 1);

  /* Spring-like */
  --ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1);
  --ease-spring-soft: cubic-bezier(0.32, 0.72, 0, 1);

  /* Smooth deceleration */
  --ease-smooth: cubic-bezier(0.4, 0, 0.2, 1);

  /* Dramatic entrance */
  --ease-dramatic: cubic-bezier(0.77, 0, 0.175, 1);

  /* Elastic bounce */
  --ease-bounce: cubic-bezier(0.68, -0.55, 0.27, 1.55);
}

/* Duration guidelines */
/* Micro (hover, toggle):     100-200ms */
/* Standard (menu, panel):    250-350ms */
/* Emphasis (modal, page):    400-600ms */
/* Dramatic (full transition): 600-1000ms */
```

---

## SITE ANALYSIS WORKFLOW

When shown a URL, follow this process to reverse-engineer the design:

### Step 1: Identify the Stack
```
Check for: framework meta tags, _nuxt/ or _next/ in network requests,
GSAP in window.gsap, Lenis via smooth scroll behavior, Three.js canvas,
Barba.js data-barba attributes, Spline viewer elements
```

### Step 2: Map Techniques Used
Cross-reference with the technique index:
- Smooth scroll present? -> T1 (Lenis) or T14 (CSS scroll-driven)
- Custom cursor? -> T3
- Text animations? -> T2 (SplitType)
- Clip-path reveals? -> T4
- Horizontal scroll? -> T5
- Page transitions? -> T10 (Barba) or T15 (View Transitions)
- 3D elements? -> T12 (Three.js) or Spline
- Micro-interactions? -> T21-T27

### Step 3: Note the Design System
- Font families (inspect body, h1-h6)
- Color palette (primary, secondary, accent, background, text)
- Spacing rhythm (section padding, grid gaps)
- Border radius patterns
- Shadow depths

### Step 4: Implementation Plan
Map each identified technique to the skill's code patterns.
Combine techniques following the performance decision matrix.

---

*GENESIS Gold Standard — The 1% details that make the difference between good and unforgettable.*
