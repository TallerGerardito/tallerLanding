# Proposal: Landing Page — Taller Gerardito

## Intent

Taller Gerardito needs a professional landing page to establish its digital presence, attract new clients searching online for automotive services in San Manuel, Cortés (Honduras), and convert visitors into WhatsApp leads or booked appointments. The current project is a fresh Astro scaffold with default boilerplate — no business content exists yet.

The landing page is the core deliverable of the tallergerardito.com website project. It must communicate the shop's competitive advantage (electronic diagnostic expertise), build trust, and provide clear calls to action.

## Scope

### In Scope
- Single-page landing with ~10 content sections built as Astro components
- GSAP + ScrollTrigger animations (hero entrance, scroll reveals, stagger effects)
- Responsive mobile-first design using Tailwind CSS 4
- Brand color system: azul oscuro (`#1e3a5f`) + naranja (`#f97316`)
- WhatsApp floating CTA button (wa.me/50489638660)
- Google Maps embed (San Manuel, Cortés location)
- SEO local: meta tags, Open Graph, Schema.org LocalBusiness JSON-LD, lang="es"
- Sticky/responsive navbar with smooth scroll navigation
- All business content sourced from `tallerInfo.txt`

### Out of Scope
- Online appointment booking system (future phase)
- Blog/content marketing section (future phase)
- Multi-page routing (single landing page only)
- CMS or admin panel
- Analytics integration (Google Analytics, Meta Pixel)
- Custom photography — will use SVG icons and placeholder images
- Domain registration and hosting setup

## Approach

**Single-Page Component Architecture**: Each landing section is an independent Astro component composed in `index.astro`. GSAP is loaded once via a `<script>` tag in the layout, using ScrollTrigger for scroll-based section animations. All styling uses Tailwind utility classes; CSS files are reserved exclusively for complex `@keyframes` animations.

### Sections
| # | Section | Component | Description |
|---|---------|-----------|-------------|
| 1 | Navbar | `Navbar.astro` | Sticky nav with logo, section links, WhatsApp CTA |
| 2 | Hero | `Hero.astro` | Business name, slogan, dual CTAs (WhatsApp + scroll to services) |
| 3 | Services | `Services.astro` | 6 service cards (mecánica, diagnóstico, transmisiones, eléctrica, electrónica, A/C) |
| 4 | About | `About.astro` | 10+ year history, mission, founder story |
| 5 | Equipment | `Equipment.astro` | Diagnostic tools showcase (escáner, osciloscopio, endoscopio, banco de pruebas) |
| 6 | Why Us | `WhyUs.astro` | 4 competitive advantages (certeza diagnóstica, garantía, precio, tecnología) |
| 7 | Brands | `Brands.astro` | Supported vehicle brands (Toyota, Nissan, Honda, etc.) |
| 8 | Testimonials | `Testimonials.astro` | Placeholder testimonial cards for future reviews |
| 9 | Contact | `Contact.astro` | Phone, WhatsApp, email, address, business hours |
| 10 | Map | `Map.astro` | Google Maps iframe embed |
| 11 | Footer | `Footer.astro` | Copyright, quick links, social media placeholders |
| — | Floating | `WhatsAppButton.astro` | Fixed-position WhatsApp button (always visible) |

### GSAP Animation Plan
- **Hero**: Title slides up + fade, subtitle staggers in, CTA buttons pop with scale
- **Services**: Cards stagger in from bottom on scroll
- **Equipment**: Tool icons animate in with rotation, counter animation for "10+ años"
- **Brands**: Logo grid stagger reveal
- **General**: All sections use `.animate-on-scroll` class with ScrollTrigger (start: "top 85%")

## Affected Areas

| Area | Impact | Description |
|------|--------|-------------|
| `src/pages/index.astro` | **Modified** | Replace boilerplate with landing page composition |
| `src/layouts/Layout.astro` | **Modified** | Add SEO meta, Schema.org JSON-LD, fonts, lang="es", GSAP script |
| `src/components/Welcome.astro` | **Removed** | Delete Astro default component |
| `src/components/Navbar.astro` | **New** | Sticky navigation bar |
| `src/components/Hero.astro` | **New** | Hero section with animated entrance |
| `src/components/Services.astro` | **New** | Service cards grid |
| `src/components/About.astro` | **New** | History and mission section |
| `src/components/Equipment.astro` | **New** | Diagnostic tools showcase |
| `src/components/WhyUs.astro` | **New** | Competitive advantages |
| `src/components/Brands.astro` | **New** | Supported vehicle brands |
| `src/components/Testimonials.astro` | **New** | Client testimonials (placeholder) |
| `src/components/Contact.astro` | **New** | Contact info and business hours |
| `src/components/Map.astro` | **New** | Google Maps embed |
| `src/components/Footer.astro` | **New** | Page footer |
| `src/components/WhatsAppButton.astro` | **New** | Floating WhatsApp CTA |
| `src/styles/global.css` | **Modified** | Add custom fonts, CSS variables, GSAP keyframes |
| `src/assets/` | **Modified** | Remove Astro defaults, add SVG icons |
| `package.json` | **Modified** | Add `gsap` dependency |

## Risks

| Risk | Likelihood | Mitigation |
|------|------------|------------|
| GSAP bundle size impacts performance | Low | GSAP core + ScrollTrigger is ~25kb gzipped; acceptable for landing page |
| No real images available | Medium | Use SVG icons (Lucide/Heroicons) and CSS gradients; placeholder structure ready for photos |
| Google Maps embed slows page load | Medium | Lazy load iframe with `loading="lazy"` attribute |
| Content in Spanish may need proofreading | Low | Content sourced directly from business document (tallerInfo.txt) |
| GSAP animations janky on low-end mobile | Low | Use `will-change` and `transform`-only animations; reduce motion for `prefers-reduced-motion` |

## Rollback Plan

Since the project is a fresh scaffold with no production content:
1. `git revert` the implementation commits to restore the Astro boilerplate
2. Remove `gsap` from `package.json` and run `npm install`
3. All changes are additive (new components) or replacements of boilerplate — no production data at risk

## Dependencies

- **GSAP** (`gsap` npm package) — must be installed before implementation
- **Google Fonts** — Inter font family (loaded via `<link>` or `@import`)
- **tallerInfo.txt** — business content source (already in project root)

## Success Criteria

- [ ] Landing page renders correctly at mobile (375px), tablet (768px), and desktop (1280px+) breakpoints
- [ ] All 10 content sections display correct business information from tallerInfo.txt
- [ ] GSAP hero animation plays on page load (title, subtitle, CTAs)
- [ ] GSAP scroll animations trigger as sections enter viewport
- [ ] WhatsApp floating button links to wa.me/50489638660
- [ ] Google Maps embed shows San Manuel, Cortés location
- [ ] SEO meta tags present: title, description, Open Graph, Schema.org LocalBusiness
- [ ] `astro build` completes without errors
- [ ] Page scores 90+ on Lighthouse performance (no heavy images)
- [ ] `prefers-reduced-motion` disables GSAP animations gracefully
