# Design: Landing Page — Taller Gerardito

## Technical Approach

Build a single-page landing site for Taller Gerardito using the existing Astro 5 + Tailwind CSS 4 scaffold. Each content section is an independent `.astro` component composed inside `src/pages/index.astro`. GSAP (with ScrollTrigger plugin) is loaded once from the layout via a CDN `<script>` tag and initialized through a single client-side script that queries `.animate-on-scroll` elements. All styling uses Tailwind v4 utility classes; CSS files are reserved exclusively for custom `@keyframes` animations. SEO is handled via `<head>` meta tags, Open Graph properties, and a Schema.org `LocalBusiness` JSON-LD block injected in the layout.

This approach maps directly to the proposal's "Single-Page Component Architecture" strategy.

## Component Hierarchy

```
Layout.astro
├── <head>
│   ├── SEO meta tags (title, description, OG, Twitter)
│   ├── Schema.org LocalBusiness JSON-LD
│   ├── Google Fonts (Inter) <link>
│   └── GSAP + ScrollTrigger CDN <script>
│
└── <body>
    └── index.astro
        ├── Navbar.astro ─────────── sticky top-0, smooth scroll links, WhatsApp CTA
        ├── Hero.astro ───────────── GSAP entrance animation (no ScrollTrigger)
        ├── Services.astro ───────── 6 service cards, stagger-in on scroll
        ├── About.astro ──────────── History, mission, founder
        ├── Equipment.astro ──────── 4 diagnostic tool cards, icon animations
        ├── WhyUs.astro ──────────── 4 advantage cards
        ├── Brands.astro ─────────── Logo grid, stagger reveal
        ├── Testimonials.astro ───── Placeholder testimonial cards
        ├── Contact.astro ────────── Phone, WhatsApp, email, hours, address
        ├── Map.astro ────────────── Google Maps iframe (lazy loaded)
        ├── Footer.astro ─────────── Copyright, quick links
        └── WhatsAppButton.astro ─── Fixed position, always visible
```

### Component Responsibility Matrix

| Component | Type | Interactive | GSAP | Data Source |
|-----------|------|------------|------|-------------|
| `Layout.astro` | Layout | No | Loads script | Props (title, description) |
| `Navbar.astro` | Static + JS | Yes (mobile menu, scroll spy) | No | Hardcoded section links |
| `Hero.astro` | Static | No | Yes (entrance) | `tallerInfo.txt` content |
| `Services.astro` | Static | No | Yes (scroll) | `tallerInfo.txt` §1.4 |
| `About.astro` | Static | No | Yes (scroll) | `tallerInfo.txt` §1.7 |
| `Equipment.astro` | Static | No | Yes (scroll) | `tallerInfo.txt` §5 |
| `WhyUs.astro` | Static | No | Yes (scroll) | `tallerInfo.txt` §5 |
| `Brands.astro` | Static | No | Yes (scroll) | `tallerInfo.txt` §1.4 |
| `Testimonials.astro` | Static | No | Yes (scroll) | Placeholder data |
| `Contact.astro` | Static | No | Yes (scroll) | `tallerInfo.txt` §1.4 |
| `Map.astro` | Static | Yes (iframe) | No | Google Maps embed URL |
| `Footer.astro` | Static | No | No | Hardcoded |
| `WhatsAppButton.astro` | Static | Yes (link) | Yes (pulse) | Hardcoded wa.me link |

## Architecture Decisions

### Decision: GSAP via CDN instead of npm bundle

**Choice**: Load GSAP core + ScrollTrigger from the official GSAP CDN (`https://cdn.jsdelivr.net/npm/gsap@3/...`) via `<script>` tags in `Layout.astro`.

**Alternatives considered**:
- Install `gsap` as npm dependency and import in Astro `<script>` tags
- Use Astro `client:load` islands with a framework (React/Svelte) for animations

**Rationale**: Astro 5 ships zero JS by default. GSAP operates on the DOM directly and does not need framework integration. Loading via CDN avoids bundler complications with GSAP's license model (Club plugins are CDN-only), keeps the Astro build pipeline clean, and leverages browser caching across pages. The CDN approach also avoids the `gsap` npm package appearing in the Vite dependency graph, which can cause tree-shaking issues with GSAP plugins. Total payload: ~28kb gzipped (core + ScrollTrigger).

### Decision: Single animation initialization script

**Choice**: One `<script>` block at the bottom of `Layout.astro` that initializes all GSAP animations using a class-based query system (`.animate-on-scroll`, `data-animate` attributes).

**Alternatives considered**:
- Per-component `<script>` tags with GSAP code in each `.astro` file
- A shared `src/scripts/animations.ts` imported by individual components

**Rationale**: GSAP's ScrollTrigger works best with a single registration point. Splitting initialization across multiple component scripts creates race conditions (ScrollTrigger registered multiple times, elements not yet in DOM). A single script at the bottom of the layout guarantees all DOM elements exist before animation initialization. Components declare their animation intent via CSS classes and `data-*` attributes; the central script reads and applies them.

### Decision: Tailwind CSS 4 with @theme for design tokens

**Choice**: Define the brand color palette, typography scale, and spacing tokens using Tailwind v4's `@theme` directive in `src/styles/global.css`.

**Alternatives considered**:
- CSS custom properties without Tailwind integration
- Tailwind v3-style `tailwind.config.js` (not applicable to v4)

**Rationale**: Tailwind v4 uses `@theme` inside CSS instead of a config file. This keeps all design tokens in one place (`global.css`), is the canonical v4 approach, and generates utility classes automatically (e.g., `bg-brand-blue`, `text-brand-orange`). No extra config files needed.

### Decision: Google Fonts via `<link>` preconnect + stylesheet

**Choice**: Load Inter font family via Google Fonts `<link>` tags in `Layout.astro` `<head>` with `preconnect` hints.

**Alternatives considered**:
- Self-hosted font files in `public/fonts/`
- `@fontsource/inter` npm package

**Rationale**: For a single landing page with no complex font loading requirements, the Google Fonts CDN is the simplest approach with excellent caching. Adding `preconnect` to `fonts.googleapis.com` and `fonts.gstatic.com` eliminates connection latency. If performance testing reveals font-loading as a bottleneck, self-hosting can be adopted later without architectural changes.

### Decision: Schema.org JSON-LD inline in Layout

**Choice**: Embed a `<script type="application/ld+json">` block directly in `Layout.astro` with the `LocalBusiness` schema, populated from hardcoded business data.

**Alternatives considered**:
- Astro content collection for business data
- External JSON file imported at build time

**Rationale**: The business data is static and unlikely to change frequently. A single JSON-LD block in the layout is the simplest approach with zero runtime cost. The data is small enough (~30 lines) that extracting it to a separate file adds indirection without meaningful benefit. If future pages need the same data, it can be refactored to a shared data file then.

### Decision: Mobile navigation via CSS + minimal JS (no framework)

**Choice**: Implement the mobile hamburger menu using a `<script>` tag within `Navbar.astro` that toggles CSS classes. No framework needed.

**Alternatives considered**:
- Astro island with React/Svelte for the nav
- CSS-only `:target` or checkbox hack for mobile menu

**Rationale**: The navbar interaction is a simple toggle (open/close menu + close on link click). A 15-line vanilla JS script is lighter than shipping a framework runtime. The CSS-only approach would work but lacks smooth transitions and scroll-spy behavior. Vanilla JS keeps the zero-framework philosophy of the project.

### Decision: Lazy-load Google Maps iframe

**Choice**: Use `loading="lazy"` on the Maps `<iframe>` and wrap it in an `IntersectionObserver`-triggered container that only renders the iframe when the section is near the viewport.

**Alternatives considered**:
- Load iframe immediately with just `loading="lazy"`
- Use Google Maps Static API (image only)

**Rationale**: Google Maps iframes are heavy (~800kb+ with all resources). The `loading="lazy"` attribute provides baseline lazy loading, but wrapping it in an IntersectionObserver gives more control over when the iframe loads. This protects the Lighthouse performance score. The Static API lacks interactivity (no zoom/pan), which reduces user experience.

## Data Flow

```
tallerInfo.txt (source of truth)
       │
       ▼  (content hardcoded into components at build time)
  ┌────────────────────────────────────────────────────┐
  │                  Layout.astro                       │
  │  ┌─────────────────────────────────────────────┐   │
  │  │ <head> SEO + JSON-LD + Fonts + GSAP CDN     │   │
  │  └─────────────────────────────────────────────┘   │
  │  ┌─────────────────────────────────────────────┐   │
  │  │ <body>                                       │   │
  │  │   index.astro                                │   │
  │  │     ├── Navbar ←── scroll events (JS)        │   │
  │  │     ├── Hero                                 │   │
  │  │     ├── Services                             │   │
  │  │     ├── About                                │   │
  │  │     ├── Equipment                            │   │
  │  │     ├── WhyUs                                │   │
  │  │     ├── Brands                               │   │
  │  │     ├── Testimonials                         │   │
  │  │     ├── Contact                              │   │
  │  │     ├── Map ←── IntersectionObserver (JS)    │   │
  │  │     └── Footer                               │   │
  │  │   WhatsAppButton (fixed position)            │   │
  │  └─────────────────────────────────────────────┘   │
  │  ┌─────────────────────────────────────────────┐   │
  │  │ <script> GSAP initialization                 │   │
  │  │   1. Register ScrollTrigger plugin           │   │
  │  │   2. Hero entrance animation (immediate)     │   │
  │  │   3. Query all [data-animate] elements       │   │
  │  │   4. Create ScrollTrigger for each           │   │
  │  │   5. Check prefers-reduced-motion            │   │
  │  └─────────────────────────────────────────────┘   │
  └────────────────────────────────────────────────────┘
```

**Animation data flow**:

```
Component HTML                GSAP Script
─────────────                 ───────────
<div data-animate="fade-up">  ──→  gsap.from(el, { y: 40, opacity: 0 })
<div data-animate="stagger">  ──→  gsap.from(children, { stagger: 0.1 })
<div data-animate="scale-in"> ──→  gsap.from(el, { scale: 0.8, opacity: 0 })
```

## Design System — Tailwind CSS 4

### Color Palette

| Token | Value | Usage |
|-------|-------|-------|
| `--color-brand-blue` | `#1e3a5f` | Primary backgrounds, headings, navbar |
| `--color-brand-blue-light` | `#2a4f7f` | Hover states, secondary backgrounds |
| `--color-brand-blue-dark` | `#152a45` | Footer, dark sections |
| `--color-brand-orange` | `#f97316` | CTAs, accents, highlights, icons |
| `--color-brand-orange-light` | `#fb923c` | Hover states on orange elements |
| `--color-neutral-50` | `#fafafa` | Page background |
| `--color-neutral-100` | `#f5f5f5` | Alternating section backgrounds |
| `--color-neutral-200` | `#e5e5e5` | Borders, dividers |
| `--color-neutral-600` | `#525252` | Body text |
| `--color-neutral-800` | `#262626` | Headings on light backgrounds |
| `--color-neutral-900` | `#171717` | Strong text |
| `--color-white` | `#ffffff` | Cards, light backgrounds |

### Typography

| Token | Value | Usage |
|-------|-------|-------|
| Font family | `Inter, system-ui, sans-serif` | All text |
| `text-hero` | `clamp(2.5rem, 5vw, 4rem)` | Hero heading |
| `text-section-title` | `clamp(1.75rem, 3vw, 2.5rem)` | Section headings |
| `text-card-title` | `1.25rem` (20px) | Card headings |
| `text-body` | `1rem` (16px) | Body text |
| `text-small` | `0.875rem` (14px) | Captions, meta text |
| Line height body | `1.6` | Body text |
| Line height headings | `1.2` | All headings |

### Tailwind v4 `@theme` Configuration

```css
/* src/styles/global.css */
@import "tailwindcss";

@theme {
  --color-brand-blue: #1e3a5f;
  --color-brand-blue-light: #2a4f7f;
  --color-brand-blue-dark: #152a45;
  --color-brand-orange: #f97316;
  --color-brand-orange-light: #fb923c;

  --font-family-sans: "Inter", system-ui, sans-serif;

  --text-hero: clamp(2.5rem, 5vw, 4rem);
  --text-section-title: clamp(1.75rem, 3vw, 2.5rem);
}
```

### Responsive Breakpoints

| Breakpoint | Tailwind Prefix | Width | Target |
|------------|----------------|-------|--------|
| Mobile (base) | — | 0–639px | Phones (375px primary) |
| Tablet | `sm:` | 640px+ | Small tablets |
| Tablet landscape | `md:` | 768px+ | Tablets, small laptops |
| Desktop | `lg:` | 1024px+ | Laptops |
| Wide desktop | `xl:` | 1280px+ | Desktop monitors |

**Layout strategy per breakpoint**:

| Section | Mobile | Tablet (md) | Desktop (lg+) |
|---------|--------|-------------|----------------|
| Navbar | Hamburger menu | Hamburger menu | Horizontal links |
| Hero | Stacked, full-width | Stacked, centered | Side-by-side text + visual |
| Services | 1 column | 2 columns | 3 columns |
| Equipment | 1 column | 2 columns | 4 columns |
| WhyUs | 1 column | 2 columns | 4 columns |
| Brands | 2 columns grid | 3 columns | 5-6 columns |
| Testimonials | 1 column | 2 columns | 3 columns |
| Contact | Stacked | 2 columns | 2 columns |
| Map | Full width | Full width | Max-width container |

## SEO / Schema.org Approach

### Meta Tags Strategy

```html
<head>
  <title>Taller Gerardito — Mecánica y Diagnóstico Electrónico Automotriz | San Manuel, Cortés</title>
  <meta name="description" content="Taller mecánico en San Manuel, Cortés con más de 10 años de experiencia. Diagnóstico electrónico especializado, mecánica general, transmisiones, A/C. Llámenos al 8963-8660." />
  <meta name="keywords" content="taller mecánico San Manuel, diagnóstico electrónico automotriz, mecánico Cortés Honduras, taller Gerardito" />
  <link rel="canonical" href="https://tallergerardito.com/" />
  <html lang="es" />

  <!-- Open Graph -->
  <meta property="og:type" content="website" />
  <meta property="og:locale" content="es_HN" />
  <meta property="og:title" content="Taller Gerardito — Diagnóstico Electrónico Automotriz" />
  <meta property="og:description" content="..." />
  <meta property="og:url" content="https://tallergerardito.com/" />
  <meta property="og:site_name" content="Taller Gerardito" />
  <meta property="og:image" content="https://tallergerardito.com/og-image.png" />

  <!-- Twitter -->
  <meta name="twitter:card" content="summary_large_image" />
</head>
```

### Schema.org LocalBusiness JSON-LD

```json
{
  "@context": "https://schema.org",
  "@type": "AutoRepair",
  "name": "Taller Gerardito",
  "description": "Taller mecánico especializado en diagnóstico electrónico automotriz en San Manuel, Cortés, Honduras.",
  "url": "https://tallergerardito.com",
  "telephone": "+50489638660",
  "email": "gerarnoe21@gmail.com",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "2 cuadras arriba del Carwash Mendoza",
    "addressLocality": "San Manuel",
    "addressRegion": "Cortés",
    "addressCountry": "HN"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": "15.4",
    "longitude": "-87.92"
  },
  "openingHoursSpecification": {
    "@type": "OpeningHoursSpecification",
    "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
    "opens": "08:00",
    "closes": "17:00"
  },
  "priceRange": "$$",
  "image": "https://tallergerardito.com/og-image.png",
  "sameAs": [],
  "founder": {
    "@type": "Person",
    "name": "Gerardo García"
  },
  "foundingDate": "2015",
  "areaServed": ["San Manuel", "Choloma", "Villanueva", "San Pedro Sula"]
}
```

The `@type` is `AutoRepair` (a Schema.org subtype of `LocalBusiness`) which is the most specific type for automotive repair shops and gives the best SEO signal to Google.

## GSAP Integration Strategy

### Loading

```html
<!-- Layout.astro <head> -->
<script src="https://cdn.jsdelivr.net/npm/gsap@3/dist/gsap.min.js" defer></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3/dist/ScrollTrigger.min.js" defer></script>
```

### Initialization Pattern

```html
<!-- Layout.astro, after </body> slot -->
<script>
  document.addEventListener("DOMContentLoaded", () => {
    // Respect reduced motion preference
    const prefersReducedMotion = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
    if (prefersReducedMotion) return;

    gsap.registerPlugin(ScrollTrigger);

    // Hero entrance (immediate, no scroll trigger)
    gsap.from("[data-animate='hero-title']", { y: 40, opacity: 0, duration: 0.8, ease: "power3.out" });
    gsap.from("[data-animate='hero-subtitle']", { y: 30, opacity: 0, duration: 0.8, delay: 0.2, ease: "power3.out" });
    gsap.from("[data-animate='hero-cta']", { scale: 0.8, opacity: 0, duration: 0.6, delay: 0.5, ease: "back.out(1.7)" });

    // Scroll-triggered sections
    document.querySelectorAll("[data-animate='fade-up']").forEach((el) => {
      gsap.from(el, {
        y: 40, opacity: 0, duration: 0.6, ease: "power2.out",
        scrollTrigger: { trigger: el, start: "top 85%" }
      });
    });

    // Stagger children (service cards, brand logos, etc.)
    document.querySelectorAll("[data-animate='stagger']").forEach((container) => {
      gsap.from(container.children, {
        y: 30, opacity: 0, duration: 0.5, stagger: 0.1, ease: "power2.out",
        scrollTrigger: { trigger: container, start: "top 85%" }
      });
    });
  });
</script>
```

### Animation Attribute Convention

Components declare animation intent via `data-animate` attributes:

| Attribute Value | Behavior | Used By |
|----------------|----------|---------|
| `hero-title` | Slide up + fade, immediate | Hero.astro |
| `hero-subtitle` | Slide up + fade, delayed | Hero.astro |
| `hero-cta` | Scale pop, delayed | Hero.astro |
| `fade-up` | Slide up + fade on scroll | Section headings, About, Contact |
| `stagger` | Children stagger in on scroll | Services, Equipment, WhyUs, Brands, Testimonials |
| `scale-in` | Scale up + fade on scroll | Equipment icons |

## File Changes

| File | Action | Description |
|------|--------|-------------|
| `src/layouts/Layout.astro` | **Modify** | Add props (title, description), SEO meta tags, OG tags, JSON-LD, Google Fonts link, GSAP CDN scripts, `lang="es"`, GSAP init script |
| `src/pages/index.astro` | **Modify** | Replace boilerplate with landing page composition importing all section components |
| `src/styles/global.css` | **Modify** | Add `@theme` block with brand colors, typography tokens; add `@keyframes` for WhatsApp pulse animation; add smooth-scroll and base body styles |
| `src/components/Welcome.astro` | **Delete** | Default Astro boilerplate, replaced by landing sections |
| `src/assets/astro.svg` | **Delete** | Default Astro asset, no longer needed |
| `src/assets/background.svg` | **Delete** | Default Astro asset, no longer needed |
| `src/components/Navbar.astro` | **Create** | Sticky nav with logo text, section links, mobile hamburger, WhatsApp CTA button |
| `src/components/Hero.astro` | **Create** | Full-viewport hero with business name, slogan, dual CTA buttons |
| `src/components/Services.astro` | **Create** | 6-card grid for services (mecánica, diagnóstico, transmisiones, eléctrica, electrónica, A/C) |
| `src/components/About.astro` | **Create** | History section with 10+ year story, mission, founder |
| `src/components/Equipment.astro` | **Create** | 4 diagnostic tool cards (escáner, osciloscopio, endoscopio, banco de pruebas) |
| `src/components/WhyUs.astro` | **Create** | 4 advantage cards (certeza diagnóstica, garantía, precio, tecnología) |
| `src/components/Brands.astro` | **Create** | Vehicle brand logo grid (Toyota, Nissan, Honda, Mazda, etc.) |
| `src/components/Testimonials.astro` | **Create** | Placeholder testimonial cards for future reviews |
| `src/components/Contact.astro` | **Create** | Contact info: phone, WhatsApp, email, address, business hours |
| `src/components/Map.astro` | **Create** | Google Maps lazy-loaded iframe embed for San Manuel location |
| `src/components/Footer.astro` | **Create** | Copyright, quick links, social media placeholders |
| `src/components/WhatsAppButton.astro` | **Create** | Fixed-position floating WhatsApp button with pulse animation |
| `public/og-image.png` | **Create** | Open Graph preview image (placeholder, 1200x630) |

**Summary**: 3 modified, 3 deleted, 13 created.

## Interfaces / Contracts

### Layout Props

```typescript
// Layout.astro frontmatter
interface Props {
  title?: string;      // Page <title>, defaults to "Taller Gerardito — Mecánica y Diagnóstico Electrónico"
  description?: string; // Meta description, defaults to standard business description
}
```

### Section ID Contracts (for Navbar smooth scroll)

Each section component MUST render a root element with a specific `id`:

| Component | Element ID | Navbar Link |
|-----------|-----------|-------------|
| Hero.astro | `#inicio` | Inicio |
| Services.astro | `#servicios` | Servicios |
| About.astro | `#nosotros` | Nosotros |
| Equipment.astro | `#equipo` | Equipo |
| WhyUs.astro | `#por-que` | ¿Por qué? |
| Brands.astro | `#marcas` | — (no nav link) |
| Testimonials.astro | `#testimonios` | — (no nav link) |
| Contact.astro | `#contacto` | Contacto |

### Animation Data Attributes Contract

Components that participate in GSAP animations MUST use `data-animate` attributes as defined in the GSAP Integration Strategy section. The central script in `Layout.astro` is the only consumer of these attributes.

## File Structure (Final State)

```
src/
├── assets/                          # (cleared of Astro defaults)
├── components/
│   ├── Navbar.astro
│   ├── Hero.astro
│   ├── Services.astro
│   ├── About.astro
│   ├── Equipment.astro
│   ├── WhyUs.astro
│   ├── Brands.astro
│   ├── Testimonials.astro
│   ├── Contact.astro
│   ├── Map.astro
│   ├── Footer.astro
│   └── WhatsAppButton.astro
├── layouts/
│   └── Layout.astro                 # SEO, fonts, GSAP, JSON-LD
├── pages/
│   └── index.astro                  # Composes all components
└── styles/
    └── global.css                   # @theme tokens, @keyframes, base styles
public/
└── og-image.png                     # OG preview placeholder
```

## Testing Strategy

| Layer | What to Test | Approach |
|-------|-------------|----------|
| Build | Compilation succeeds | Run `astro build` — must complete with zero errors |
| Visual | All 10 sections render correctly | Manual browser check at 375px, 768px, 1280px |
| SEO | Meta tags, JSON-LD present | View page source; validate JSON-LD with Google Rich Results Test |
| Performance | Lighthouse score 90+ | Run Lighthouse in Chrome DevTools (mobile + desktop) |
| Accessibility | `prefers-reduced-motion` | Toggle reduced motion in OS settings; verify no GSAP animations play |
| Links | WhatsApp button works | Click floating button; verify opens `wa.me/50489638660` |
| Links | Navbar smooth scroll | Click each nav link; verify smooth scroll to correct section |
| Responsive | Mobile hamburger menu | Test open/close toggle on mobile viewport |
| Maps | Iframe loads | Scroll to map section; verify Google Maps loads after IntersectionObserver trigger |

## Migration / Rollout

No migration required. This is a greenfield landing page replacing Astro boilerplate on a project with no production deployment. All changes are additive (new components) or replacements of default scaffold files. Rollback is a simple `git revert`.

## Open Questions

- [ ] Exact Google Maps coordinates for Taller Gerardito in San Manuel, Cortés (currently estimated at ~15.4, -87.92) — needs confirmation from business owner
- [ ] Open Graph image (`og-image.png`) — should a branded placeholder be designed, or use a generic automotive graphic?
- [ ] Exact vehicle brand list for `Brands.astro` — proposal lists Toyota, Nissan, Mazda, Honda, Mitsubishi, Hyundai, Jeep, Ford, Chevrolet; confirm if complete
