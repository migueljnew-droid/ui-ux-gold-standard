# GENESIS UI/UX Gold Standard

The ultimate UI/UX design skill for Claude Code. 28 award-winning techniques, 5 engines, 24 CSV databases with 1,334+ entries.

Built by reverse-engineering 9 Awwwards SOTY-level websites and combining a BM25 design intelligence engine with production-ready code patterns.

## What This Skill Does

When activated, Claude Code becomes an elite UI/UX designer that:
- **Generates data-driven design systems** — colors, typography, styles matched to your industry using 161 reasoning rules
- **Builds Awwwards-level interfaces** — 28 production-ready techniques from smooth scroll to WebGL shaders
- **Uses modern CSS-native patterns first** — Scroll-Driven Animations, View Transitions, Container Queries before reaching for JS
- **Adds premium micro-interactions** — button state machines, 3D card tilt, spring-physics drawers, toast systems
- **Reverse-engineers any website** — structured 4-step workflow to identify tech stack and map to technique library

## The 5 Engines

| Engine | File | What It Does |
|--------|------|-------------|
| **Data Engine** | `scripts/` + `data/` | BM25 search across 24 CSV databases (67 styles, 161 palettes, 57 font pairings, 99 UX guidelines, 161 reasoning rules, 13 stacks) |
| **Creative Engine** | `SKILL.md` | Anti-Gravity $10K design philosophy, Spline 3D integration, Vercel deploy |
| **Techniques Engine** | `TECHNIQUES.md` | T1-T13: Lenis, GSAP ScrollTrigger, SplitType, Three.js, Barba.js, custom cursors |
| **Modern CSS Engine** | `TECHNIQUES_MODERN.md` | T14-T20: CSS Scroll-Driven Animations, View Transitions API, Container Queries, :has(), Popover API, Rive, Advanced GSAP (Flip, MotionPath, DrawSVG, MorphSVG) |
| **Micro-Interaction Engine** | `TECHNIQUES_MICRO.md` | T21-T28: Button choreography, input animations, card effects, nav patterns, toast systems, modal/drawer physics, loading states, WebGL shaders, premium timing functions |

## 28 Techniques

### Foundation (T1-T13)
| # | Technique | Libraries |
|---|-----------|-----------|
| 1 | Lenis + GSAP ScrollTrigger foundation | lenis, gsap |
| 2 | Text splitting animations | split-type, gsap |
| 3 | Custom cursor with mix-blend-mode: difference | Vanilla JS |
| 4 | Clip-path scroll reveals | gsap |
| 5 | Horizontal scroll sections | gsap |
| 6 | Gooey/liquid filter effect | CSS/SVG |
| 7 | Variable font animations | CSS @property |
| 8 | Theme-aware navigation | IntersectionObserver |
| 9 | Image displacement/distortion | three.js |
| 10 | Page transitions | barba.js, gsap |
| 11 | Grayscale to color reveals | CSS filters |
| 12 | Three.js post-processing | three.js |
| 13 | Marquee/infinite scroll text | CSS keyframes |

### Modern CSS-Native + Advanced (T14-T20)
| # | Technique | Libraries |
|---|-----------|-----------|
| 14 | CSS Scroll-Driven Animations | Native CSS |
| 15 | View Transitions API | Native CSS/JS |
| 16 | Container Queries | Native CSS |
| 17 | :has() relational selector | Native CSS |
| 18 | Popover API + Anchor Positioning | Native HTML/CSS |
| 19 | Rive animations | @rive-app |
| 20 | Advanced GSAP (Flip, MotionPath, DrawSVG, MorphSVG) | gsap (Club) |

### Micro-Interactions & Components (T21-T28)
| # | Technique | Libraries |
|---|-----------|-----------|
| 21 | Button micro-interactions | CSS/JS |
| 22 | Input & form animations | CSS |
| 23 | Card hover effects (3D tilt, spotlight, glow) | CSS/JS |
| 24 | Navigation choreography | CSS/JS |
| 25 | Toast & notification animations | CSS/JS |
| 26 | Modal & drawer choreography | CSS |
| 27 | Loading & skeleton patterns | CSS/JS |
| 28 | WebGL shader gallery | three.js/CSS |

## Installation

### Claude Code Skill (Recommended)
```bash
# Copy to your Claude Code skills directory
cp -r . ~/.claude/skills/ui-ux-pro-max/
```

The skill auto-activates on any UI/UX request — designing pages, choosing colors, building components, reviewing interfaces.

### Design System Generator (CLI)
```bash
# Generate a complete design system
python3 scripts/search.py "SaaS analytics dashboard" --design-system -p "MetricFlow"

# Search specific domains
python3 scripts/search.py "glassmorphism dark" --domain style
python3 scripts/search.py "elegant luxury" --domain typography
python3 scripts/search.py "animation accessibility" --domain ux

# Stack-specific guidelines
python3 scripts/search.py "performance" --stack nextjs
```

## Data Coverage

| Database | Entries | What It Contains |
|----------|---------|-----------------|
| Products | 95 | Product type to style mapping |
| Styles | 67 | UI styles with 22 attributes each |
| Colors | 96 | Industry-matched color palettes |
| Typography | 56 | Font pairings with Google Fonts URLs |
| UX Guidelines | 98 | Best practices with code examples |
| UI Reasoning | 100 | Industry-specific design rules |
| Icons | 100 | Icon library recommendations |
| Charts | 25 | Data visualization guidance |
| Landing | 30 | Page structure patterns |
| React Performance | 44 | React/Next.js optimization |
| Web Interface | 30 | Web accessibility guidelines |
| Stack Guides | 13 files | Framework-specific best practices |

## Award-Winning Stack

Based on studying 9 Awwwards SOTY-level sites:

| Layer | Tool | Usage |
|-------|------|-------|
| Smooth Scroll | Lenis | 4/9 winners |
| Animation | GSAP + ScrollTrigger | 5/9 winners |
| 3D | Three.js | 3/9 winners |
| Framework | Nuxt.js or Next.js | 5/9 winners |
| CMS | Sanity / Storyblok | 6/9 winners |

## Sites Studied

landonorris.com, igloo.inc (SOTY 2024), messenger.abeto.co, dontboardme.com, kprverse.com, synchronized.studio, stitchingstories.nl, beckyentertainment.co, manayerbamate.com

## Philosophy

> Every interface should feel like it cost $10,000+.

The Gold Standard Test: "Would this feel out of place on a $10K agency portfolio?" If yes, push harder.

## License

MIT

---

*Built by Louis Gold. Powered by GENESIS.*
