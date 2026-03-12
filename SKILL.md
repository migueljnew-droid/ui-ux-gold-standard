---
name: ui-ux-pro-max
description: "GENESIS UI/UX Gold Standard. 28 award-winning techniques, 5 engines, 24 CSV databases. Design intelligence + creative execution + modern CSS-native + micro-interactions. 67 styles, 161 palettes, 57 font pairings, 25 charts, 99 UX guidelines, 161 reasoning rules, 13 stacks. Anti-Gravity $10K design philosophy. Spline 3D, Rive, Three.js, GSAP Flip/MotionPath/MorphSVG/DrawSVG. CSS Scroll-Driven Animations, View Transitions API, Container Queries, :has() selector, Popover API. Vercel deploy. Actions: plan, build, create, design, implement, review, fix, improve, optimize, enhance, refactor, check, analyze UI/UX code. Projects: website, landing page, dashboard, admin panel, e-commerce, SaaS, portfolio, blog, mobile app, .html, .tsx, .vue, .svelte. Elements: button, modal, navbar, sidebar, card, table, form, chart, drawer, toast, skeleton. Styles: glassmorphism, claymorphism, minimalism, brutalism, neumorphism, bento grid, dark mode, responsive, skeuomorphism, flat design, retro-futuristic, maximalist, editorial, art deco. Topics: color palette, accessibility, animation, layout, typography, font pairing, spacing, hover, shadow, gradient, 3D, Spline, Rive, deploy, scroll animation, page transition, micro-interaction, WebGL, shader."
---

# GENESIS UI/UX Gold Standard

The Gold Standard for UI/UX. Combines a BM25 search engine (67 styles, 161 color palettes, 57 font pairings, 99 UX guidelines, 161 industry reasoning rules, 13 tech stacks) with Anti-Gravity creative philosophy — every interface should feel like it cost $10,000+.

**Five engines, one skill:**
1. **Data Engine** — Industry-aware design system generator with BM25 ranking across 24 CSV databases
2. **Creative Engine** — Bold aesthetic direction, Spline 3D, cinematic motion, typography-as-identity
3. **Techniques Engine** — 13 foundational techniques. See `skills/ui-ux-pro-max/TECHNIQUES.md`
4. **Modern CSS Engine** — 7 CSS-native + advanced techniques. See `skills/ui-ux-pro-max/TECHNIQUES_MODERN.md`
5. **Micro-Interaction Engine** — 8 component choreography patterns. See `skills/ui-ux-pro-max/TECHNIQUES_MICRO.md`

### 28 Award-Winning Techniques

When building Awwwards-level sites, read the technique files for production-ready code patterns:

**TECHNIQUES.md — Foundation (T1-T13)**

| # | Technique | Key Libraries |
|---|-----------|---------------|
| 1 | Lenis + GSAP ScrollTrigger foundation | lenis, gsap |
| 2 | Text splitting animations | split-type, gsap |
| 3 | Custom cursor with mix-blend-mode: difference | Vanilla JS |
| 4 | Clip-path scroll reveals (polygon, circle, ellipse, inset) | gsap |
| 5 | Horizontal scroll sections (pin + translateX) | gsap |
| 6 | Gooey/liquid filter effect (blur + contrast) | CSS/SVG |
| 7 | Variable font animations (@property) | CSS |
| 8 | Theme-aware navigation (dark/light swap) | IntersectionObserver |
| 9 | Image displacement/distortion (WebGL shaders) | three.js |
| 10 | Page transitions (clip-path wipes) | barba.js, gsap |
| 11 | Grayscale to color reveals | CSS filters |
| 12 | Three.js post-processing (chromatic aberration, grain) | three.js |
| 13 | Marquee/infinite scroll text | CSS keyframes |

**TECHNIQUES_MODERN.md — CSS-Native + Advanced (T14-T20)**

| # | Technique | Key Libraries |
|---|-----------|---------------|
| 14 | CSS Scroll-Driven Animations (zero-JS scroll effects) | Native CSS |
| 15 | View Transitions API (native page transitions) | Native CSS/JS |
| 16 | Container Queries (component-level responsive) | Native CSS |
| 17 | :has() relational selector (parent styling) | Native CSS |
| 18 | Popover API + Anchor Positioning (native modals) | Native HTML/CSS |
| 19 | Rive animations (interactive state machines) | @rive-app |
| 20 | Advanced GSAP (Flip, MotionPath, DrawSVG, MorphSVG, ScrollSmoother) | gsap (Club) |

**TECHNIQUES_MICRO.md — Micro-Interactions & Components (T21-T28)**

| # | Technique | Key Libraries |
|---|-----------|---------------|
| 21 | Button micro-interactions (sweep, lift, state machine) | CSS/JS |
| 22 | Input & form animations (floating label, validation) | CSS |
| 23 | Card hover effects (3D tilt, spotlight, border glow) | CSS/JS |
| 24 | Navigation choreography (transform, menu reveal, sliding indicator) | CSS/JS |
| 25 | Toast & notification animations (slide-in, auto-dismiss) | CSS/JS |
| 26 | Modal & drawer choreography (scale-from-origin, spring physics) | CSS |
| 27 | Loading & skeleton patterns (shimmer, blur-up, crossfade) | CSS/JS |
| 28 | WebGL shader gallery (noise, gradient mesh, metaballs) | three.js/CSS |

**Also includes:** Premium timing functions (8 cubic-bezier curves), Site Analysis Workflow, CSS vs JS decision matrix, performance tiering

**Stack from 9 studied award winners:** Nuxt.js (5/9), GSAP (5/9), Lenis (4/9), Three.js (3/9), headless CMS (6/9)

---

## When to Apply

Activate on ANY UI/UX request:
- Designing new UI components or pages
- Choosing color palettes and typography
- Reviewing code for UX issues
- Building landing pages, dashboards, apps
- Implementing accessibility requirements
- Creating distinctive, memorable interfaces

---

## Phase 1: Design Thinking (BEFORE Code)

Before touching code, commit to a direction. Generic is the enemy.

### Context Analysis
- **Purpose**: What problem does this interface solve? Who uses it?
- **Tone**: Pick an extreme — brutally minimal, maximalist chaos, retro-futuristic, organic/natural, luxury/refined, playful/toy-like, editorial/magazine, brutalist/raw, art deco/geometric, soft/pastel, industrial/utilitarian
- **Constraints**: Framework, performance, accessibility requirements
- **Differentiation**: What makes this UNFORGETTABLE? What's the one thing someone will remember?

### The Gold Standard Test
Every design must pass: "Would this feel out of place on a $10K agency portfolio?" If yes, push harder.

---

## Phase 2: Design System Generation (REQUIRED)

### Step 1: Generate Design System

**Always start here.** The engine searches 5 domains in parallel (product, style, color, landing, typography), applies 161 industry reasoning rules, and returns a complete system.

```bash
python3 skills/ui-ux-pro-max/scripts/search.py "<product_type> <industry> <keywords>" --design-system [-p "Project Name"]
```

**Examples:**
```bash
# SaaS dashboard
python3 skills/ui-ux-pro-max/scripts/search.py "SaaS analytics dashboard professional" --design-system -p "MetricFlow"

# Beauty/wellness landing page
python3 skills/ui-ux-pro-max/scripts/search.py "beauty spa wellness service elegant" --design-system -p "Serenity Spa"

# Fintech crypto app
python3 skills/ui-ux-pro-max/scripts/search.py "fintech crypto trading dark" --design-system -p "CryptoVault"
```

### Step 2: Persist Design System (Optional — Multi-Page Projects)

Save for hierarchical retrieval across sessions:

```bash
# Save master design system
python3 skills/ui-ux-pro-max/scripts/search.py "<query>" --design-system --persist -p "Project Name"

# With page-specific override
python3 skills/ui-ux-pro-max/scripts/search.py "<query>" --design-system --persist -p "Project Name" --page "dashboard"
```

Creates:
- `design-system/MASTER.md` — Global Source of Truth
- `design-system/pages/*.md` — Page-specific overrides

**Retrieval rule:** Check `pages/[page].md` first. If exists, its rules override MASTER. Otherwise, use MASTER exclusively.

### Step 3: Supplemental Domain Searches

After the design system, drill deeper as needed:

```bash
python3 skills/ui-ux-pro-max/scripts/search.py "<keyword>" --domain <domain> [-n <max_results>]
```

| Need | Domain | Example |
|------|--------|---------|
| More style options | `style` | `--domain style "glassmorphism dark"` |
| Chart recommendations | `chart` | `--domain chart "real-time dashboard"` |
| UX best practices | `ux` | `--domain ux "animation accessibility"` |
| Alternative fonts | `typography` | `--domain typography "elegant luxury"` |
| Landing structure | `landing` | `--domain landing "hero social-proof"` |
| React performance | `react` | `--domain react "suspense rerender"` |
| Web accessibility | `web` | `--domain web "aria focus keyboard"` |
| Icon recommendations | `icons` | `--domain icons "navigation action"` |

### Step 4: Stack Guidelines

Get implementation-specific best practices. Default to `html-tailwind` if unspecified.

```bash
python3 skills/ui-ux-pro-max/scripts/search.py "<keyword>" --stack html-tailwind
```

**Available stacks:** `html-tailwind`, `react`, `nextjs`, `vue`, `nuxtjs`, `nuxt-ui`, `svelte`, `astro`, `swiftui`, `react-native`, `flutter`, `shadcn`, `jetpack-compose`

---

## Phase 3: Creative Execution (The Gold Standard)

### Typography as Identity

The font IS the brand. Never default to generic fonts.

**BANNED:** Inter, Roboto, Arial, system fonts, Open Sans — these scream "template"

**USE INSTEAD:** Distinctive fonts from [Fontshare](https://fontshare.com):
- Display: Clash Display, Satoshi, Cabinet Grotesk, General Sans, Switzer
- Pair a distinctive display font with a refined body font
- The design system generator suggests pairings — but always push for something unexpected

**Rule:** If two different projects could use the same font pairing, at least one of them is wrong.

### Color & Theme

- Commit to a cohesive aesthetic. Use CSS variables for consistency
- Dominant colors with sharp accents outperform timid, evenly-distributed palettes
- The design system provides industry-matched palettes — use them as a foundation, then push contrast

### Motion & Animation

High-impact moments over scattered micro-interactions:

- **Page load:** One well-orchestrated entrance with staggered reveals (`animation-delay`) creates more delight than 20 scattered animations
- **Scroll triggers:** Elements that earn their entrance
- **Hover states:** Should surprise, not just confirm
- **Timing:** 150-300ms for micro-interactions. Use CSS-only solutions for HTML. Motion library for React.
- **ALWAYS** check `prefers-reduced-motion`

### Spatial Composition

- Unexpected layouts. Asymmetry. Overlap. Diagonal flow
- Grid-breaking elements
- Generous negative space OR controlled density — never boring middle ground
- Content flows like a narrative, not a spreadsheet

### Backgrounds & Atmosphere

Create depth. Never default to flat solid colors:
- Gradient meshes, noise textures, geometric patterns
- Layered transparencies, dramatic shadows
- Grain overlays, decorative borders
- Custom cursors for interactive moments

### Anti-Generic Mandate

**NEVER** produce:
- Overused font families (Inter, Roboto, Arial)
- Cliched color schemes (purple gradients on white)
- Predictable layouts and component patterns
- Cookie-cutter design lacking context-specific character
- Same aesthetic across different projects (vary light/dark, fonts, styles)

**ALWAYS** interpret creatively. Make unexpected choices that feel genuinely designed for the context.

---

## Phase 4: Spline 3D Integration

When the project calls for interactive 3D scenes (hero backgrounds, product showcases, immersive experiences), use [Spline.design](https://spline.design).

### Embed by Stack

| Stack | Method |
|---|---|
| Vanilla HTML/JS | `<spline-viewer>` web component OR `@splinetool/runtime` |
| React / Vite | `@splinetool/react-spline` |
| Next.js | `@splinetool/react-spline/next` |
| Vue | `@splinetool/vue-spline` |
| iframe | Public URL iframe |

### Setup Checklist (BEFORE embedding)

User must: Spline editor → **Export** → **Code Export** → copy `prod.spline.design` URL. Check:
- Toggle **Hide Background** ON (if site has custom bg)
- Toggle **Hide Spline Logo** ON (paid plan)
- Set **Geometry Quality** to Performance
- Disable **Page Scroll**, **Zoom**, **Pan** unless needed
- Click **Generate Draft** / **Promote to Production** after changes

### React / Vite (Lazy Loading)
```jsx
import { lazy, Suspense } from 'react';
const Spline = lazy(() => import('@splinetool/react-spline'));

export default function Hero() {
  return (
    <Suspense fallback={<div style={{ background: '#0a0a0a', width: '100%', height: '100vh' }} />}>
      <Spline scene="https://prod.spline.design/REPLACE_ME/scene.splinecode" />
    </Suspense>
  );
}
```

### Next.js (SSR Fix)
```jsx
import dynamic from 'next/dynamic';
const Spline = dynamic(() => import('@splinetool/react-spline/next'), {
  ssr: false,
  loading: () => <div style={{ background: '#0a0a0a', width: '100%', height: '100vh' }} />
});
export default function Page() {
  return <Spline scene="https://prod.spline.design/REPLACE_ME/scene.splinecode" />;
}
```

### Vanilla HTML
```html
<script type="module" src="https://unpkg.com/@splinetool/viewer/build/spline-viewer.js"></script>
<spline-viewer url="https://prod.spline.design/REPLACE_ME/scene.splinecode" background="transparent" events-target="global"></spline-viewer>
```

### Full-Page 3D Hero (Production Pattern)
```jsx
import Spline from '@splinetool/react-spline';
import { useState } from 'react';

export default function HeroSection() {
  const [loaded, setLoaded] = useState(false);
  const isMobile = typeof window !== 'undefined' && window.innerWidth < 768;
  const isLowEnd = typeof navigator !== 'undefined' && navigator.hardwareConcurrency <= 2;

  return (
    <div style={{ position: 'relative', width: '100vw', height: '100vh' }}>
      <div style={{
        position: 'absolute', inset: 0, background: '#0a0a0a', zIndex: 0,
        opacity: loaded ? 0 : 1, transition: 'opacity 0.5s ease'
      }} />
      {!isMobile && !isLowEnd && (
        <Spline
          scene="https://prod.spline.design/REPLACE_ME/scene.splinecode"
          onLoad={() => setLoaded(true)}
          style={{ position: 'absolute', top: 0, left: 0, width: '100%', height: '100%', zIndex: 0 }}
        />
      )}
      <div style={{ position: 'relative', zIndex: 1 }}>
        <h1>Your Content Here</h1>
      </div>
    </div>
  );
}
```

### Runtime API — Object Control
```js
spline.load(sceneUrl).then(() => {
  const obj = spline.findObjectByName('MyObject');
  obj.position.x += 50;
  obj.rotation.y += Math.PI / 2;    // RADIANS not degrees
  obj.scale.x = 2; obj.scale.y = 2; obj.scale.z = 2;
  obj.emitEvent('mouseHover');       // trigger animation
  obj.emitEventReverse('mouseHover'); // reverse
  spline.setVariable('score', spline.getVariable('score') + 1);
  spline.addEventListener('mouseDown', (e) => console.log('Clicked:', e.target.name));
});
```

### Spline Event Types

| Event | Use case |
|---|---|
| `mouseDown` | Click/tap on object |
| `mouseUp` | Release after click |
| `mouseHover` | Cursor enters object area |
| `mousePress` | Holding click down |
| `keyDown` / `keyUp` | Key press/release |
| `start` | Scene loaded and started |
| `scroll` | Page scrolled |

### Scene Performance Limits
- Under 3MB = fine
- 3-10MB = optimize
- Over 10MB = serious problem
- Over 20MB = export as video, don't embed

### Mobile Strategy (ALWAYS implement one)

**Option A — Skip on mobile (recommended):**
```js
if (window.innerWidth < 768 || navigator.hardwareConcurrency <= 2) {
  // Show static background or video instead
}
```

**Option B — Export as video for mobile:** Record animation as MP4, serve on mobile.

### CLS Prevention
```css
spline-viewer, canvas.spline-canvas {
  display: block; width: 100%; height: 100vh;
  contain: strict;
}
```

### Load Timeout Fallback (ALWAYS add)
```js
const TIMEOUT_MS = 8000;
const timeoutId = setTimeout(() => {
  document.getElementById('spline-fallback').style.display = 'block';
  document.querySelector('.spline-wrapper').style.display = 'none';
}, TIMEOUT_MS);
spline.load(sceneUrl).then(() => clearTimeout(timeoutId));
```

### Common Gotchas

| Symptom | Fix |
|---|---|
| Page won't scroll | `body { overflow: auto !important }` or disable Page Scroll in Play Settings |
| White box behind scene | Play Settings → Hide Background → regenerate URL |
| Laggy on non-Mac | Add `navigator.hardwareConcurrency` check, skip on low-end |
| Buttons not clickable | `pointer-events: none` on Spline wrapper; `pointer-events: all` on content |
| Hydration error (Next.js) | `dynamic(() => import(...), { ssr: false })` |
| Rotations look wrong | Use `Math.PI / 180 * degrees` |
| CORS error | Download `.splinecode` and self-host |

---

## Phase 5: Instant Ship — Vercel Deployment

Zero-friction shipping. Code to live URL in seconds.

### Vercel MCP (Preferred)
If configured, use Vercel MCP directly for deploy, domain management, project config.

### Vercel CLI Fallback
```bash
npm i -g vercel
vercel              # preview deploy
vercel --prod       # production deploy
```

### Framework Detection
- **Next.js**: Zero-config on Vercel
- **React/Vite**: Output `dist/`, framework Vite
- **Vanilla HTML**: Output project root or `public/`
- **Astro/Remix/SvelteKit**: Auto-detected

### Deploy Checklist
- [ ] Build passes locally
- [ ] Env vars set in Vercel
- [ ] Preview deployment tested before production
- [ ] Custom domain configured if needed

---

## UX Rule Categories by Priority

| Priority | Category | Impact |
|----------|----------|--------|
| 1 | Accessibility | CRITICAL |
| 2 | Touch & Interaction | CRITICAL |
| 3 | Performance | HIGH |
| 4 | Layout & Responsive | HIGH |
| 5 | Typography & Color | MEDIUM |
| 6 | Animation | MEDIUM |
| 7 | Style Selection | MEDIUM |
| 8 | Charts & Data | LOW |

### 1. Accessibility (CRITICAL)
- `color-contrast` — Minimum 4.5:1 ratio for normal text
- `focus-states` — Visible focus rings on interactive elements
- `alt-text` — Descriptive alt text for meaningful images
- `aria-labels` — aria-label for icon-only buttons
- `keyboard-nav` — Tab order matches visual order
- `form-labels` — Use label with for attribute

### 2. Touch & Interaction (CRITICAL)
- `touch-target-size` — Minimum 44x44px touch targets
- `hover-vs-tap` — Use click/tap for primary interactions
- `loading-buttons` — Disable button during async operations
- `error-feedback` — Clear error messages near problem
- `cursor-pointer` — Add cursor-pointer to clickable elements

### 3. Performance (HIGH)
- `image-optimization` — Use WebP, srcset, lazy loading
- `reduced-motion` — Check prefers-reduced-motion
- `content-jumping` — Reserve space for async content

### 4. Layout & Responsive (HIGH)
- `viewport-meta` — width=device-width initial-scale=1
- `readable-font-size` — Minimum 16px body text on mobile
- `horizontal-scroll` — Ensure content fits viewport width
- `z-index-management` — Define z-index scale (10, 20, 30, 50)

---

## Common Mistakes That Ruin Professional UI

### Icons & Visual Elements
| Do | Don't |
|----|----- |
| Use SVG icons (Heroicons, Lucide, Simple Icons) | Use emojis as UI icons |
| Use fixed viewBox (24x24) with w-6 h-6 | Mix different icon sizes |
| Research official brand SVGs from Simple Icons | Guess logo paths |
| Use color/opacity transitions on hover | Use scale transforms that shift layout |

### Interaction & Cursor
| Do | Don't |
|----|----- |
| `cursor-pointer` on all clickable elements | Leave default cursor on interactive elements |
| `transition-colors duration-200` | Instant state changes or >500ms |
| Visual feedback (color, shadow, border) on hover | No indication element is interactive |

### Light/Dark Mode Contrast
| Do | Don't |
|----|----- |
| `bg-white/80` for glass cards in light mode | `bg-white/10` (too transparent) |
| `#0F172A` (slate-900) for text | `#94A3B8` (slate-400) for body text |
| `#475569` (slate-600) minimum for muted text | gray-400 or lighter |
| `border-gray-200` in light mode | `border-white/10` (invisible) |

### Layout & Spacing
| Do | Don't |
|----|----- |
| `top-4 left-4 right-4` for floating navbar | `top-0 left-0 right-0` |
| Account for fixed navbar height | Let content hide behind fixed elements |
| Same `max-w-6xl` or `max-w-7xl` throughout | Mix container widths |

---

## Pre-Delivery Checklist

### Visual Quality
- [ ] No emojis used as icons (SVG only)
- [ ] All icons from consistent set (Heroicons/Lucide)
- [ ] Brand logos verified (Simple Icons)
- [ ] Hover states don't cause layout shift
- [ ] Theme colors used directly (`bg-primary`) not `var()` wrapper

### Interaction
- [ ] All clickable elements have `cursor-pointer`
- [ ] Hover states provide clear visual feedback
- [ ] Transitions 150-300ms
- [ ] Focus states visible for keyboard navigation

### Light/Dark Mode
- [ ] Light mode text has 4.5:1 minimum contrast
- [ ] Glass/transparent elements visible in light mode
- [ ] Borders visible in both modes
- [ ] Both modes tested before delivery

### Layout
- [ ] Floating elements have proper edge spacing
- [ ] No content hidden behind fixed navbars
- [ ] Responsive at 375px, 768px, 1024px, 1440px
- [ ] No horizontal scroll on mobile

### Accessibility
- [ ] All images have alt text
- [ ] Form inputs have labels
- [ ] Color is not the only indicator
- [ ] `prefers-reduced-motion` respected

### Gold Standard
- [ ] Design feels intentional, not templated
- [ ] Typography is distinctive, not generic
- [ ] At least one memorable creative choice
- [ ] Atmosphere created through depth/texture/motion

---

## The Build Workflow (Summary)

1. **Design direction** → Bold aesthetic commitment (Phase 1)
2. **Design system** → Data-driven recommendations (Phase 2)
3. **Structure** → Layout, typography system, color tokens
4. **Atmosphere** → Backgrounds, textures, gradients, grain (T28: gradient mesh, metaballs)
5. **Motion** → Page load sequence, scroll triggers, hover states (T1-T7, T14: CSS scroll-driven)
6. **Micro-interactions** → Buttons, inputs, cards, nav, toasts (T21-T27)
7. **3D/Animation layer** → Spline, Rive (T19), Three.js (T12) where they add value
8. **Page transitions** → View Transitions API (T15) or Barba.js (T10)
9. **Performance** → CSS-first decision matrix, device tiering, lazy loading
10. **Validate** → Pre-delivery checklist + site analysis workflow
11. **Ship** → Vercel deploy, preview URL, iterate (Phase 5)

---

## Search Reference

### Available Domains

| Domain | Use For |
|--------|---------|
| `product` | Product type recommendations |
| `style` | UI styles, colors, effects |
| `typography` | Font pairings, Google Fonts |
| `color` | Color palettes by product type |
| `landing` | Page structure, CTA strategies |
| `chart` | Chart types, library recommendations |
| `ux` | Best practices, anti-patterns |
| `icons` | Icon library recommendations |
| `react` | React/Next.js performance |
| `web` | Web interface guidelines |
| `prompt` | AI prompts, CSS keywords |

### Output Formats

```bash
# ASCII box (default)
python3 skills/ui-ux-pro-max/scripts/search.py "fintech crypto" --design-system

# Markdown
python3 skills/ui-ux-pro-max/scripts/search.py "fintech crypto" --design-system -f markdown

# JSON
python3 skills/ui-ux-pro-max/scripts/search.py "fintech" --domain product --json
```

---

## Tips for Better Results

1. **Be specific** — "healthcare SaaS dashboard" > "app"
2. **Search multiple times** — Different keywords reveal different insights
3. **Combine domains** — Style + Typography + Color = Complete system
4. **Always check UX** — Search "animation", "z-index", "accessibility"
5. **Use stack flag** — Get implementation-specific best practices
6. **Push the creative** — The data engine gives you the foundation; the Gold Standard demands you elevate it

---

*GENESIS UI/UX Gold Standard — Data-Driven Design Intelligence + Anti-Gravity Creative Execution*
