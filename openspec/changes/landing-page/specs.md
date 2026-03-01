# Landing Page Specification — Taller Gerardito

## Purpose

This specification defines the behavioral requirements for the Taller Gerardito single-page landing website (tallergerardito.com). The landing page serves as the primary digital channel for the automotive repair shop located in San Manuel, Cortes, Honduras, converting online visitors into WhatsApp leads and walk-in customers. All content is in Spanish and sourced from the business document (tallerInfo.txt).

---

## 1. Page Structure and Composition

### Requirement: Single-Page Section Composition

The landing page MUST be composed of exactly 10 content sections plus a navbar, footer, and floating WhatsApp button, rendered in a single scrollable page with no client-side routing.

#### Scenario: All sections render in correct order

- GIVEN the user navigates to the root URL (/)
- WHEN the page finishes loading
- THEN the page MUST display sections in this order: Navbar, Hero, Services, About, Equipment, Why Us, Brands, Testimonials, Contact, Map, Footer
- AND a floating WhatsApp button MUST be visible overlaying the page

#### Scenario: No multi-page routing exists

- GIVEN the Astro project is built
- WHEN inspecting the output
- THEN only a single `index.html` page MUST exist (no other route pages)

### Requirement: Component Independence

Each landing section MUST be implemented as an independent Astro component that can be rendered in isolation without dependencies on other section components.

#### Scenario: Section renders without sibling sections

- GIVEN any single section component (e.g., Services.astro)
- WHEN it is included in a page without other section components
- THEN it MUST render without errors

---

## 2. Navbar

### Requirement: Sticky Navigation Bar

The navbar MUST remain fixed at the top of the viewport during scrolling and MUST contain the business logo/name, navigation links to each section, and a WhatsApp CTA button.

#### Scenario: Navbar stays visible on scroll

- GIVEN the user is viewing any section of the page
- WHEN the user scrolls down past the hero section
- THEN the navbar MUST remain fixed at the top of the viewport
- AND the navbar MUST have a visible background to ensure readability over content

#### Scenario: Navigation links scroll to corresponding sections

- GIVEN the navbar is visible with section links
- WHEN the user clicks a navigation link (e.g., "Servicios")
- THEN the page MUST smoothly scroll to the corresponding section
- AND the target section MUST be visible below the navbar (not hidden behind it)

#### Scenario: WhatsApp CTA in navbar

- GIVEN the navbar is rendered
- WHEN the user views the navbar
- THEN a WhatsApp CTA button or link MUST be visible
- AND clicking it MUST open `https://wa.me/50489638660`

#### Scenario: Navbar collapses on mobile

- GIVEN the viewport width is less than 768px
- WHEN the user views the navbar
- THEN the navigation links SHOULD collapse into a hamburger menu or equivalent mobile pattern
- AND the WhatsApp CTA MUST remain accessible

---

## 3. Hero Section

### Requirement: Hero Content Display

The hero section MUST display the business name "Taller Gerardito", the slogan "Con la mejor preparacion en sistemas de encendido electronico", and two call-to-action buttons.

#### Scenario: Hero content is correct

- GIVEN the hero section is rendered
- WHEN the user views it
- THEN the business name "Taller Gerardito" MUST be visible as the primary heading
- AND the slogan MUST be displayed as secondary text
- AND the location "San Manuel, Cortes" SHOULD be visible

#### Scenario: Dual CTA buttons are present

- GIVEN the hero section is rendered
- WHEN the user views the CTA area
- THEN a WhatsApp CTA button MUST be present linking to `https://wa.me/50489638660`
- AND a secondary CTA button MUST scroll the user to the Services section

### Requirement: Hero GSAP Entrance Animation

The hero section MUST animate on page load using GSAP with a title slide-up, subtitle stagger, and CTA button scale-pop effect.

#### Scenario: Hero animates on page load

- GIVEN the page is loaded for the first time
- WHEN the hero section becomes visible
- THEN the title MUST animate by sliding up and fading in
- AND the subtitle MUST animate with a stagger effect
- AND the CTA buttons MUST animate with a scale/pop effect
- AND all animations MUST complete within 2 seconds of page load

#### Scenario: Hero animation respects reduced motion

- GIVEN the user has `prefers-reduced-motion: reduce` enabled in their OS
- WHEN the page loads
- THEN the hero content MUST be visible immediately without animation

---

## 4. Services Section

### Requirement: Service Cards Display

The services section MUST display exactly 6 service cards representing the core services: mecanica general, diagnostico electronico, transmisiones, electricidad automotriz, electronica avanzada, and aire acondicionado automotriz.

#### Scenario: All six services are displayed

- GIVEN the services section is rendered
- WHEN the user views it
- THEN exactly 6 service cards MUST be visible
- AND each card MUST contain a title, a brief description, and an icon or visual indicator

#### Scenario: Service content matches business data

- GIVEN the services section is rendered
- WHEN reviewing the service card content
- THEN the services listed MUST match those described in the business document: mecanica general, diagnostico electronico garantizado (motor gasolina y diesel), transmisiones automaticas y mecanicas, electricidad automotriz, electronica avanzada, and aire acondicionado automotriz

### Requirement: Services Scroll Animation

The service cards MUST animate into view using GSAP ScrollTrigger with a stagger effect from the bottom.

#### Scenario: Service cards stagger in on scroll

- GIVEN the services section is not yet in the viewport
- WHEN the user scrolls until the section top reaches 85% of the viewport height
- THEN the service cards MUST animate in from the bottom with a stagger delay between each card

#### Scenario: Animation disabled for reduced motion

- GIVEN the user has `prefers-reduced-motion: reduce` enabled
- WHEN the services section enters the viewport
- THEN all service cards MUST be visible immediately without animation

---

## 5. About Section

### Requirement: Business History and Mission

The about section MUST communicate the business history (10+ years), the founder story (Gerardo Garcia), the mission statement, and the team composition (3 people).

#### Scenario: About content includes key business facts

- GIVEN the about section is rendered
- WHEN the user reads the content
- THEN the section MUST mention "mas de 10 anos de experiencia"
- AND MUST reference the founder Gerardo Garcia
- AND MUST include the mission statement or a summary of it
- AND SHOULD mention the team of 3 people

#### Scenario: About section includes the vision

- GIVEN the about section is rendered
- WHEN the user reviews the content
- THEN the section SHOULD include the vision of being recognized by 2030 as the most trusted shop in the Cortes industrial corridor

---

## 6. Equipment Section

### Requirement: Diagnostic Tools Showcase

The equipment section MUST showcase the 4 key diagnostic tools that differentiate Taller Gerardito: escaner automotriz, osciloscopio, camara endoscopica, and banco de pruebas.

#### Scenario: All four diagnostic tools are displayed

- GIVEN the equipment section is rendered
- WHEN the user views it
- THEN exactly 4 equipment items MUST be displayed
- AND each item MUST have a name, description, and visual element (icon or image placeholder)

#### Scenario: Equipment content matches business data

- GIVEN the equipment section is rendered
- WHEN reviewing the equipment descriptions
- THEN the escaner MUST be described as "multimarca para lectura de codigos OBD"
- AND the osciloscopio MUST be described for "analisis de senales electricas en tiempo real"
- AND the camara endoscopica MUST be described for "inspeccion interna de motores sin desarmar"
- AND the banco de pruebas MUST be described for "inyectores, alternadores y componentes electricos"

### Requirement: Equipment GSAP Animation

The equipment section MUST use GSAP ScrollTrigger to animate tool icons with rotation effects on scroll entry.

#### Scenario: Equipment icons animate on scroll

- GIVEN the equipment section is not in the viewport
- WHEN the user scrolls until it enters view
- THEN the tool icons/visuals MUST animate in with a rotation effect
- AND the "10+ anos" indicator SHOULD animate with a counter effect

#### Scenario: Equipment animation respects reduced motion

- GIVEN the user has `prefers-reduced-motion: reduce` enabled
- WHEN the equipment section enters the viewport
- THEN all equipment items MUST be visible immediately without animation

---

## 7. Why Us Section

### Requirement: Competitive Advantages Display

The Why Us section MUST present exactly 4 competitive advantages: certeza diagnostica (diagnostic certainty), garantia formal de servicio (service warranty), precio accesible (accessible pricing), and tecnologia especializada (specialized technology).

#### Scenario: All four advantages are displayed

- GIVEN the Why Us section is rendered
- WHEN the user views it
- THEN exactly 4 advantage items MUST be displayed
- AND each item MUST have a title, description, and visual element

#### Scenario: Advantages reflect business positioning

- GIVEN the Why Us section is rendered
- WHEN reviewing the content
- THEN the diagnostic certainty advantage MUST convey that the shop eliminates the costly trial-and-error method
- AND the warranty advantage MUST mention a formal documented warranty policy
- AND the pricing advantage MUST position the shop as "medio-accesible" compared to SPS shops
- AND the technology advantage MUST reference the specialized diagnostic equipment

---

## 8. Brands Section

### Requirement: Supported Vehicle Brands

The brands section MUST display the vehicle brands serviced by the shop: Toyota, Nissan, Mazda, Honda, Mitsubishi, Hyundai, Jeep, Ford, and Chevrolet (at minimum).

#### Scenario: All supported brands are displayed

- GIVEN the brands section is rendered
- WHEN the user views it
- THEN logos or visual representations for at least Toyota, Nissan, Mazda, Honda, Mitsubishi, Hyundai, Jeep, Ford, and Chevrolet MUST be displayed
- AND additional brands MAY be included with a "y mas" indicator

### Requirement: Brands Scroll Animation

The brand logos MUST animate with a GSAP ScrollTrigger stagger reveal effect.

#### Scenario: Brand logos stagger in on scroll

- GIVEN the brands section is not in the viewport
- WHEN the user scrolls until it enters view
- THEN the brand logos MUST appear with a stagger reveal animation

#### Scenario: Brands animation respects reduced motion

- GIVEN the user has `prefers-reduced-motion: reduce` enabled
- WHEN the brands section enters the viewport
- THEN all brand logos MUST be visible immediately without animation

---

## 9. Testimonials Section

### Requirement: Placeholder Testimonial Cards

The testimonials section MUST display placeholder testimonial cards structured to be replaced with real customer reviews in the future.

#### Scenario: Testimonial placeholders are present

- GIVEN the testimonials section is rendered
- WHEN the user views it
- THEN at least 3 placeholder testimonial cards MUST be displayed
- AND each card MUST have a name, a star rating visual, and a review text placeholder

#### Scenario: Testimonials indicate call to action for reviews

- GIVEN the testimonials section is rendered
- WHEN the user views it
- THEN the section SHOULD include a prompt encouraging customers to leave reviews on Google Maps

---

## 10. Contact Section

### Requirement: Complete Contact Information

The contact section MUST display all business contact details: phone number, WhatsApp number, email address, physical address, and business hours.

#### Scenario: All contact details are present

- GIVEN the contact section is rendered
- WHEN the user views it
- THEN the phone/WhatsApp number "8963-8660" MUST be displayed
- AND the email "gerarnoe21@gmail.com" MUST be displayed
- AND the physical address "San Manuel, Cortes, 2 cuadras arriba del Carwash Mendoza" MUST be displayed
- AND the business hours "Lunes a Sabado, 8:00 a.m. a 5:00 p.m." MUST be displayed

#### Scenario: Contact phone number is clickable

- GIVEN the contact section is rendered
- WHEN the user taps/clicks the phone number
- THEN it MUST initiate a phone call via `tel:` protocol

#### Scenario: Contact WhatsApp link works

- GIVEN the contact section is rendered
- WHEN the user clicks the WhatsApp contact option
- THEN it MUST open `https://wa.me/50489638660`

#### Scenario: Contact email is clickable

- GIVEN the contact section is rendered
- WHEN the user clicks the email address
- THEN it MUST open the default email client via `mailto:gerarnoe21@gmail.com`

---

## 11. Map Section

### Requirement: Google Maps Embed

The map section MUST display an embedded Google Maps iframe showing the Taller Gerardito location in San Manuel, Cortes, Honduras.

#### Scenario: Map iframe renders correctly

- GIVEN the map section is rendered
- WHEN the user views it
- THEN a Google Maps iframe MUST be visible showing the San Manuel, Cortes area

#### Scenario: Map iframe loads lazily

- GIVEN the map section is below the viewport on initial page load
- WHEN the page loads
- THEN the Google Maps iframe MUST have `loading="lazy"` to defer loading until the user scrolls near it

#### Scenario: Map is interactive

- GIVEN the Google Maps iframe is loaded
- WHEN the user interacts with the map (pan, zoom)
- THEN the map MUST respond to those interactions

---

## 12. Footer

### Requirement: Footer Content

The footer MUST display copyright information, quick navigation links, and social media placeholders.

#### Scenario: Footer shows copyright

- GIVEN the footer is rendered
- WHEN the user views it
- THEN the footer MUST display copyright text including "Taller Gerardito" and the current year

#### Scenario: Footer contains quick links

- GIVEN the footer is rendered
- WHEN the user views it
- THEN the footer MUST include links to key sections (Servicios, Nosotros, Contacto at minimum)
- AND clicking a link MUST smooth-scroll to the corresponding section

#### Scenario: Footer contains social media placeholders

- GIVEN the footer is rendered
- WHEN the user views it
- THEN the footer SHOULD display placeholder icons for Facebook and Instagram social profiles

---

## 13. Floating WhatsApp Button

### Requirement: Persistent WhatsApp CTA

A floating WhatsApp button MUST be visible at all times in a fixed position on the screen, providing a direct link to the business WhatsApp.

#### Scenario: WhatsApp button is always visible

- GIVEN the user is on any part of the page
- WHEN the user scrolls to any position
- THEN the floating WhatsApp button MUST remain visible in a fixed position (bottom-right corner)

#### Scenario: WhatsApp button links correctly

- GIVEN the floating WhatsApp button is visible
- WHEN the user clicks/taps it
- THEN it MUST open `https://wa.me/50489638660` in a new tab or the WhatsApp app

#### Scenario: WhatsApp button does not obstruct content

- GIVEN the floating WhatsApp button is rendered
- WHEN the user views the page on a mobile device (375px width)
- THEN the button MUST NOT overlap critical content or navigation elements
- AND the button SHOULD have adequate spacing from screen edges

---

## 14. GSAP Animations — General Behavior

### Requirement: ScrollTrigger-Based Section Animations

All content sections (excluding Navbar, Footer, and WhatsApp button) MUST use GSAP ScrollTrigger to trigger entrance animations when the section enters the viewport.

#### Scenario: Sections animate on scroll entry

- GIVEN any content section is below the viewport
- WHEN the user scrolls until the section top reaches approximately 85% of the viewport height
- THEN the section content MUST begin its entrance animation

#### Scenario: Animations play only once

- GIVEN a section has already played its entrance animation
- WHEN the user scrolls away and returns to the section
- THEN the animation SHOULD NOT replay (elements remain in their final animated state)

### Requirement: Reduced Motion Accessibility

All GSAP animations MUST be disabled when the user has `prefers-reduced-motion: reduce` enabled at the OS level.

#### Scenario: All animations disabled for reduced motion preference

- GIVEN the user has `prefers-reduced-motion: reduce` enabled
- WHEN the page loads and the user scrolls through all sections
- THEN no GSAP animations MUST play
- AND all content MUST be visible in its final state immediately

### Requirement: GSAP Performance

GSAP animations MUST use only `transform` and `opacity` properties to ensure GPU-accelerated rendering and avoid layout thrashing.

#### Scenario: Animations use performant properties only

- GIVEN any GSAP animation is defined
- WHEN inspecting the animated CSS properties
- THEN only `transform` (translate, scale, rotate) and `opacity` properties MUST be animated
- AND properties that trigger layout recalculation (width, height, top, left, margin, padding) MUST NOT be animated

---

## 15. Responsive Design

### Requirement: Mobile-First Responsive Layout

The landing page MUST be designed mobile-first and MUST render correctly at three breakpoints: mobile (375px), tablet (768px), and desktop (1280px+).

#### Scenario: Page renders correctly on mobile (375px)

- GIVEN the viewport width is 375px
- WHEN the page is rendered
- THEN all sections MUST be fully visible without horizontal scrolling
- AND text MUST be readable without zooming
- AND interactive elements (buttons, links) MUST have a minimum touch target of 44x44px

#### Scenario: Page renders correctly on tablet (768px)

- GIVEN the viewport width is 768px
- WHEN the page is rendered
- THEN the layout SHOULD adapt to use more horizontal space (e.g., 2-column grids for service cards)
- AND all content MUST remain readable and accessible

#### Scenario: Page renders correctly on desktop (1280px+)

- GIVEN the viewport width is 1280px or greater
- WHEN the page is rendered
- THEN the layout SHOULD use full desktop width with appropriate max-width constraints
- AND service cards, equipment items, and brand logos SHOULD display in multi-column grid layouts

#### Scenario: Images and embeds scale responsively

- GIVEN the page contains icons, SVGs, and the Google Maps iframe
- WHEN the viewport is resized between mobile and desktop
- THEN all visual elements MUST scale proportionally without overflow or distortion

---

## 16. SEO and Metadata

### Requirement: HTML Meta Tags

The page MUST include essential SEO meta tags: title, description, canonical URL, and language attribute.

#### Scenario: Page has correct meta tags

- GIVEN the page HTML is rendered
- WHEN inspecting the `<head>` element
- THEN a `<title>` tag MUST be present containing "Taller Gerardito" and relevant keywords
- AND a `<meta name="description">` MUST be present with a description of the business and services
- AND the `<html>` tag MUST have `lang="es"`

### Requirement: Open Graph Tags

The page MUST include Open Graph meta tags for proper social media sharing.

#### Scenario: Open Graph tags are present

- GIVEN the page HTML is rendered
- WHEN inspecting the `<head>` element
- THEN `og:title`, `og:description`, `og:type`, and `og:url` meta tags MUST be present
- AND `og:image` SHOULD be present with a relevant image URL

### Requirement: Schema.org LocalBusiness JSON-LD

The page MUST include a Schema.org LocalBusiness JSON-LD structured data block for rich search results.

#### Scenario: JSON-LD is valid and complete

- GIVEN the page HTML is rendered
- WHEN inspecting the `<head>` or `<body>` for a `<script type="application/ld+json">` tag
- THEN a valid JSON-LD block MUST be present with `@type` set to "AutoRepair" or "LocalBusiness"
- AND it MUST include: `name` ("Taller Gerardito"), `address` (San Manuel, Cortes, Honduras), `telephone` ("+50489638660"), `openingHours` ("Mo-Sa 08:00-17:00")
- AND it SHOULD include: `description`, `geo` coordinates, `url`, `priceRange`, and `areaServed`

#### Scenario: JSON-LD parses without errors

- GIVEN the JSON-LD script is present in the page
- WHEN validated with Google's Rich Results Test or a JSON-LD validator
- THEN it MUST parse without syntax errors

---

## 17. Brand Design System

### Requirement: Color Palette

The landing page MUST use the brand color system: azul oscuro (`#1e3a5f`) as the primary color and naranja (`#f97316`) as the accent color.

#### Scenario: Primary color is applied consistently

- GIVEN the page is rendered
- WHEN inspecting major UI elements (navbar, headings, footer)
- THEN the primary color azul oscuro (`#1e3a5f`) or a close derivative MUST be used as the dominant brand color

#### Scenario: Accent color highlights CTAs

- GIVEN the page is rendered
- WHEN inspecting CTA buttons and interactive highlights
- THEN the accent color naranja (`#f97316`) or a close derivative MUST be used for primary action buttons and attention-drawing elements

### Requirement: Typography

The page MUST use the Inter font family loaded from Google Fonts.

#### Scenario: Inter font is loaded and applied

- GIVEN the page is rendered
- WHEN inspecting the computed font-family of body text
- THEN the Inter font MUST be applied
- AND a system font fallback stack MUST be defined

---

## 18. Accessibility

### Requirement: Semantic HTML Structure

The page MUST use semantic HTML5 elements for document structure.

#### Scenario: Correct use of semantic elements

- GIVEN the page HTML is rendered
- WHEN inspecting the document structure
- THEN the page MUST use `<header>` for the navbar, `<main>` for the content area, `<section>` for each content section, and `<footer>` for the footer
- AND each section MUST have a heading element (h2 or h3)

### Requirement: Keyboard Navigation

All interactive elements MUST be accessible via keyboard navigation.

#### Scenario: Tab navigation reaches all interactive elements

- GIVEN the user is navigating with keyboard only
- WHEN the user presses Tab repeatedly
- THEN focus MUST move through all links, buttons, and interactive elements in logical order
- AND focused elements MUST have a visible focus indicator

### Requirement: Image Accessibility

All visual elements MUST have appropriate alternative text or ARIA labels.

#### Scenario: Icons and images have alt text

- GIVEN the page contains SVG icons, images, or decorative elements
- WHEN inspecting them for accessibility
- THEN informative images MUST have descriptive `alt` text
- AND decorative images MUST have `alt=""` or `aria-hidden="true"`

### Requirement: Color Contrast

Text elements MUST meet WCAG 2.1 AA minimum contrast ratios.

#### Scenario: Text is readable against backgrounds

- GIVEN any text element on the page
- WHEN measuring its color contrast ratio against its background
- THEN normal text MUST have a contrast ratio of at least 4.5:1
- AND large text (18px+ or 14px+ bold) MUST have a contrast ratio of at least 3:1

---

## 19. Performance

### Requirement: Build Success

The landing page MUST build successfully with `astro build` without errors.

#### Scenario: Build completes without errors

- GIVEN the full landing page implementation
- WHEN running `astro build`
- THEN the build MUST complete with exit code 0
- AND no TypeScript or template errors MUST be present

### Requirement: Lighthouse Performance Score

The landing page SHOULD achieve a Lighthouse performance score of 90 or higher.

#### Scenario: Lighthouse score meets threshold

- GIVEN the built landing page is served
- WHEN running a Lighthouse audit in performance mode
- THEN the performance score SHOULD be 90 or higher
- AND the page SHOULD have no render-blocking resources beyond critical CSS

### Requirement: GSAP Bundle Size

The GSAP library (core + ScrollTrigger) MUST NOT exceed 30KB gzipped to maintain acceptable load performance.

#### Scenario: GSAP bundle is within size budget

- GIVEN `gsap` and `ScrollTrigger` are included in the project
- WHEN measuring the gzipped transfer size
- THEN the combined size MUST NOT exceed 30KB gzipped

---

## 20. Content Integrity

### Requirement: Business Data Accuracy

All business information displayed on the landing page MUST match the data provided in `tallerInfo.txt`.

#### Scenario: Phone number is accurate

- GIVEN any display of the business phone number
- WHEN reading the number
- THEN it MUST be "8963-8660" or its international format "+504 8963-8660"

#### Scenario: Email is accurate

- GIVEN any display of the business email
- WHEN reading the email
- THEN it MUST be "gerarnoe21@gmail.com"

#### Scenario: Address is accurate

- GIVEN any display of the business address
- WHEN reading the address
- THEN it MUST include "San Manuel, Cortes" and SHOULD include "2 cuadras arriba del Carwash Mendoza"

#### Scenario: Business hours are accurate

- GIVEN any display of the business hours
- WHEN reading the hours
- THEN it MUST state "Lunes a Sabado" and "8:00 a.m. a 5:00 p.m."

#### Scenario: WhatsApp link uses correct number

- GIVEN any WhatsApp link on the page (navbar, hero, floating button, contact section)
- WHEN inspecting the href attribute
- THEN it MUST point to `https://wa.me/50489638660`
