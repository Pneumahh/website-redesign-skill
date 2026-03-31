---
name: website-redesign
description: "End-to-end website redesign and revamp skill. Use when the user wants to redesign, revamp, rebuild, or improve a website's UI/UX. Covers: design system creation, component sourcing from 21st.dev Magic MCP, section-by-section build with Framer Motion animations, Tailwind CSS styling, shadcn/ui integration, responsive design, and UX polish. Works with Next.js, React, or any modern frontend stack. Triggers on: redesign, revamp, rebuild UI, improve design, new homepage, landing page, website makeover, modernize website, refresh UI."
user-invocable: true
---

# Website Redesign Skill

A comprehensive, phased workflow for redesigning any website from discovery to delivery. Integrates design intelligence (`/ui-ux-pro-max`), visual quality (`/frontend-design`), premium components (21st.dev Magic MCP), and motion design (Framer Motion).

---

## Workflow Overview

| Phase | What | Key Tools/Skills |
|-------|------|-----------------|
| 1. Discovery | Understand the site, content, brand, tech stack | Read files, Explore agent |
| 2. Design System | Colors, typography, spacing, tokens | `/ui-ux-pro-max --design-system` |
| 3. Component Sourcing | Find & build premium UI components | `mcp__magic__21st_magic_component_*` |
| 4. Section Build | Build each page section | Framer Motion, shadcn/ui |
| 5. Animation & Motion | Scroll effects, hover states, transitions | `motion/react`, CSS transitions |
| 6. UX Polish | Accessibility, responsive, interaction quality | `/frontend-design`, `/ui-ux-pro-max` |
| 7. Assets | Images, videos, logos, icons | Unsplash, Mixkit, Lucide |
| 8. Review | Build check, visual QA, pre-delivery audit | Preview tools, checklist |

---

## Phase 1: Discovery

Before writing any code, understand the project completely.

### 1.1 Read the Content/Specs Document
- Look for a specs file (e.g., `websiteInfo.md`, `brief.md`, `README.md`) containing:
  - Brand name, tagline, mission statement
  - Brand colors (primary, secondary, accent)
  - Target audience and tone
  - Page sections and content (headings, descriptions, CTAs)
  - Product/service data (courses, pricing, features, etc.)
  - Contact info, social links
  - Partner/client logos

### 1.2 Analyze the Existing Codebase
- Identify the tech stack: Next.js version, React, Tailwind version, TypeScript
- Check for shadcn/ui (`/components/ui/`, `cn()` utility, `class-variance-authority`)
- Find the CSS entry point (`globals.css`, `app.css`) and existing theme tokens
- Check `tailwind.config.ts` or `@theme inline` block for Tailwind v4
- Identify the component directory structure (`/components/sections/`, `/components/ui/`)
- Check `next.config.ts` for image remote patterns

### 1.3 Identify What Exists vs. What Needs Building
- List existing sections/components that can be kept or adapted
- Identify gaps — sections that need to be built from scratch
- Note any external dependencies already installed (framer-motion, react-icons, etc.)

---

## Phase 2: Design System

Establish the visual foundation before building any components.

### 2.1 Generate Design Recommendations
Invoke the UI/UX skill to get comprehensive design guidance:

```
/ui-ux-pro-max
```

Then run the design system generator with the project context:
```bash
python3 <ui-ux-pro-max-scripts>/search.py "<industry> <style-keywords>" --design-system -p "Project Name"
```

### 2.2 Define Brand Tokens
Set up CSS custom properties in the project's global stylesheet. See `references/design-system-guide.md` for the full token setup pattern including:
- Color tokens (primary, heading, body, border, background)
- Shadow tokens (card, elevated)
- Font configuration
- Utility classes (`.bg-brand-gradient`, `.text-brand-gradient`)

### 2.3 Button Design Rules
- Use **solid brand color** for CTA buttons — never muddy gradients
- Define hover (darken 10%), active (darken 15% + scale 0.98), and glow shadow states
- Add a `brand` variant to the shadcn Button component
- All transitions: 200ms ease

---

## Phase 3: Component Sourcing

Use 21st.dev Magic MCP to find and build premium components.

### 3.1 Browse Inspiration
```
mcp__magic__21st_magic_component_inspiration
```
Search for component types needed: hero sections, testimonial carousels, animated tabs, card grids, sliders, etc.

### 3.2 Build Components
```
mcp__magic__21st_magic_component_builder
```
Generate components that match the design system. Provide brand colors and style preferences in the prompt.

### 3.3 Refine Components
```
mcp__magic__21st_magic_component_refiner
```
Polish generated components — adjust spacing, colors, animations to match the brand.

### 3.4 Adapt for the Project
When integrating MCP-generated components:
- Replace generic shadcn tokens (`text-muted-foreground`, `bg-secondary`) with project-specific tokens if they're not defined in the theme
- Add `"use client"` directive for components using hooks or browser APIs
- Ensure imports match the project's path aliases (`@/components/ui/`, `@/lib/utils`)
- Test with `next build` after each integration

See `references/component-integration.md` for detailed patterns.

---

## Phase 4: Section-by-Section Build

Build the page in order, section by section. See `references/section-playbook.md` for detailed patterns for each section type.

### Build Order (Recommended)
1. **Navigation/Header** — Fixed navbar with scroll-aware state changes
2. **Hero Section** — Video/image background, headline, CTAs, partner logo slider
3. **Value Proposition** — Success pathway, step cards, feature highlights
4. **Feature Sections** — Two-column layouts (text + image, alternating)
5. **Product/Service Grid** — Category tabs + filtered card grid
6. **Social Proof** — Testimonials carousel, stats/counters, partner logos
7. **Community/Gallery** — Image carousels, user-generated content
8. **Footer** — Multi-column links, contact, social, copyright

### Key Patterns
- **Scroll-aware navbar**: 3 states — transparent (top), dark frosted glass (hero), white with shadow (content). Use `window.scrollY` with a ref to the hero section's bottom edge for precise transition.
- **Category tabs**: Use AnimatedTabs with clip-path sliding animation. Build tabs dynamically from data categories.
- **Two-column features**: Alternating layout (`reversed` prop), badge label, heading, description, animated CTA button, image with hover zoom.
- **Card grids**: Responsive 1/2/3 columns, hover lift + scale, badge system, hover-reveal info pills.

---

## Phase 5: Animation & Motion

### 5.1 Framer Motion Patterns
```tsx
// Scroll-triggered entrance
<motion.div
  initial={{ opacity: 0, y: 30 }}
  whileInView={{ opacity: 1, y: 0 }}
  viewport={{ once: true }}
  transition={{ delay: index * 0.1, duration: 0.5 }}
/>

// Staggered children
<motion.div variants={{ hidden: {}, visible: { transition: { staggerChildren: 0.1 } } }}>
  <motion.div variants={{ hidden: { opacity: 0, y: 20 }, visible: { opacity: 1, y: 0 } }} />
</motion.div>

// Content swap with AnimatePresence
<AnimatePresence mode="wait">
  <motion.div key={activeKey} initial={{ opacity: 0 }} animate={{ opacity: 1 }} exit={{ opacity: 0 }} />
</AnimatePresence>
```

### 5.2 CSS Transitions
- Buttons: `transition: background 200ms ease, box-shadow 200ms ease, transform 150ms ease`
- Cards: `transition-all duration-500 hover:-translate-y-2 hover:shadow-elevated hover:scale-[1.02]`
- Images: `transition-transform duration-700 ease-out group-hover:scale-110`
- Nav links: `duration-200` for color changes
- Tab content: 150ms fade-out → swap data → fade-in

### 5.3 Micro-interactions
- Arrow icons that translate-x on hover (`group-hover:translate-x-1`)
- Button active state: `scale(0.98)` press effect
- Card image zoom on hover within `overflow-hidden` container

---

## Phase 6: UX Polish

### 6.1 Invoke Design Skills
```
/frontend-design
```
Use for visual quality review — typography choices, color contrast, spatial composition, motion quality.

```
/ui-ux-pro-max
```
Use for UX audit — search specific domains:
```bash
python3 <scripts>/search.py "accessibility animation" --domain ux
python3 <scripts>/search.py "layout responsive" --stack html-tailwind
```

### 6.2 Responsive Design
- Test at: 375px (mobile), 768px (tablet), 1024px (laptop), 1440px (desktop)
- Grid columns: 1 → 2 → 3 (mobile → tablet → desktop)
- Typography: scale down headings on mobile (`text-[2rem] md:text-[2.5rem] lg:text-[3rem]`)
- Navigation: hamburger menu on mobile with slide-in panel
- Touch targets: minimum 44x44px

### 6.3 Consistency Checks
- Uniform `border-radius` across all image containers (pick one: `rounded-lg` recommended)
- Consistent shadow tokens (`shadow-card` for rest, `shadow-elevated` for hover)
- Same spacing rhythm (4px base: gap-2, gap-4, gap-6, gap-8)
- Same section padding pattern (`py-14 md:py-20` or `py-16 md:py-24`)

---

## Phase 7: Assets

### 7.1 Images
- Use **Unsplash** for placeholder/stock images: `https://images.unsplash.com/photo-ID?w=600&h=400&fit=crop&q=80`
- Configure `next.config.ts` remote patterns for all image domains
- Use `next/image` with `fill` + `object-cover` for responsive images
- Add descriptive `alt` text for all images

### 7.2 Videos
- **Mixkit** (`https://mixkit.co/free-stock-video/`) — free, no watermark, 720p
- **Pexels** (`https://pexels.com/search/videos/`) — free, 4K available
- Video backgrounds: `autoPlay loop muted playsInline` with low opacity (40-50%)
- Direct MP4 URLs: `https://assets.mixkit.co/videos/{ID}/{ID}-720.mp4`

### 7.3 Icons
- Use **Lucide React** (`lucide-react`) for all SVG icons — never emojis
- Consistent sizing: `h-4 w-4` (inline), `h-5 w-5` (buttons), `h-6 w-6` (feature icons)

### 7.4 Logos
- Source partner/client logos from the actual website when possible
- Use `InfiniteSlider` component for logo carousels with `ProgressiveBlur` edge fades

---

## Phase 8: Review & Delivery

### 8.1 Build Verification
```bash
npx next build
```
Must pass with zero errors. Fix any TypeScript issues, missing imports, or unused variables.

### 8.2 Visual Verification
Use preview tools to verify each section:
- Start dev server and screenshot key sections
- Check console for errors
- Test interactive elements (tabs, buttons, navigation)
- Test responsive behavior at key breakpoints

### 8.3 Pre-Delivery Checklist
See `references/pre-delivery-checklist.md` for the full audit. Key items:
- [ ] No emojis used as icons (use Lucide SVGs)
- [ ] `cursor-pointer` on all clickable elements
- [ ] Hover transitions are smooth (150-300ms)
- [ ] Text contrast 4.5:1 minimum
- [ ] Focus states visible for keyboard navigation
- [ ] `prefers-reduced-motion` respected
- [ ] Responsive at 375px, 768px, 1024px, 1440px
- [ ] No horizontal scroll on mobile
- [ ] All images have alt text
- [ ] Build passes with no errors

---

## Quick Reference: When to Use What

| Need | Tool/Skill | Command |
|------|-----------|---------|
| Design system & color palette | ui-ux-pro-max | `/ui-ux-pro-max` then `--design-system` |
| UX guidelines & best practices | ui-ux-pro-max | `--domain ux` search |
| Visual quality & typography | frontend-design | `/frontend-design` |
| Browse component inspiration | Magic MCP | `mcp__magic__21st_magic_component_inspiration` |
| Generate new components | Magic MCP | `mcp__magic__21st_magic_component_builder` |
| Refine existing components | Magic MCP | `mcp__magic__21st_magic_component_refiner` |
| Scroll animations | Framer Motion | `motion/react` — `whileInView`, `AnimatePresence` |
| Stock images | Unsplash | `images.unsplash.com/photo-ID?w=600&h=400&fit=crop` |
| Stock videos | Mixkit | `assets.mixkit.co/videos/{ID}/{ID}-720.mp4` |
| SVG icons | Lucide | `lucide-react` |
| Logo search | Magic MCP | `mcp__magic__logo_search` |
