# Pre-Delivery Checklist

Run through this checklist before delivering any redesigned website.

---

## Visual Quality

- [ ] No emojis used as icons — use Lucide SVGs (`lucide-react`) everywhere
- [ ] Consistent icon set — don't mix icon libraries (pick one: Lucide recommended)
- [ ] Uniform `border-radius` on all image containers (e.g., `rounded-lg` across the site)
- [ ] Consistent shadow tokens: `shadow-card` at rest, `shadow-elevated` on hover
- [ ] Same spacing rhythm throughout (4px base: gap-2, gap-4, gap-6, gap-8)
- [ ] Same section padding pattern (e.g., `py-14 md:py-20` or `py-16 md:py-24`)
- [ ] Hover states don't shift layout (no `whileHover: { scale }` on cards — use CSS `transform` instead)
- [ ] No muddy gradients on buttons — use solid brand colors with hover/active states
- [ ] Text gradient (`.text-brand-gradient`) used only on headings, never on buttons
- [ ] Typography scale is consistent (see design-system-guide.md)

---

## Interaction Quality

- [ ] `cursor-pointer` on ALL clickable elements (buttons, links, cards, tabs, arrows)
- [ ] Hover transitions are smooth: 150-300ms duration
- [ ] Button states: hover (darken 10%), active (darken 15% + `scale(0.98)`), glow shadow
- [ ] Arrow icons translate on hover: `group-hover:translate-x-1`
- [ ] Card hover: `-translate-y-1` or `-translate-y-2` lift + `shadow-elevated`
- [ ] Image zoom on hover within `overflow-hidden` container
- [ ] Tab content transitions: fade-out → swap → fade-in (use `AnimatePresence mode="wait"`)
- [ ] Scroll animations use `viewport={{ once: true }}` to prevent re-triggering

---

## Responsive Design

- [ ] Test at 375px (mobile), 768px (tablet), 1024px (laptop), 1440px (desktop)
- [ ] No horizontal scroll on any viewport
- [ ] Grid columns adapt: 1 → 2 → 3 (mobile → tablet → desktop)
- [ ] Headings scale down on mobile: `text-[2rem] md:text-[2.5rem] lg:text-[3rem]`
- [ ] Navigation collapses to hamburger on mobile (< 768px)
- [ ] Touch targets are minimum 44x44px
- [ ] Cards are full-width on mobile, maintain readable widths on desktop
- [ ] Images maintain aspect ratio across breakpoints (use `fill` + `object-cover`)
- [ ] Horizontal carousels are touch-scrollable with `snap-x snap-mandatory`

---

## Accessibility

- [ ] Text contrast ratio minimum 4.5:1 (use WebAIM contrast checker)
- [ ] All images have descriptive `alt` text
- [ ] Form inputs have associated labels
- [ ] Focus states are visible for keyboard navigation
- [ ] `aria-label` on icon-only buttons (e.g., arrow navigation, hamburger menu)
- [ ] `prefers-reduced-motion` respected — disable autoplay and entrance animations
- [ ] Keyboard navigation works for carousels (ArrowLeft/ArrowRight)
- [ ] Skip-to-content link for keyboard users (optional but recommended)

---

## Performance

- [ ] Images use `next/image` with appropriate `sizes` prop
- [ ] Images use `priority` only for above-the-fold content (hero image)
- [ ] Lazy loading for below-the-fold images (default behavior with `next/image`)
- [ ] Video backgrounds use low-resolution sources (720p max)
- [ ] Videos have `muted playsInline autoPlay loop` attributes
- [ ] No unnecessary re-renders — memoize expensive computations
- [ ] Framer Motion animations use `will-change: transform` implicitly (no manual addition needed)
- [ ] External image domains configured in `next.config.ts` `remotePatterns`

---

## Build & Code Quality

- [ ] `npx next build` passes with zero errors
- [ ] No TypeScript errors or warnings
- [ ] No unused imports or variables
- [ ] No console.log statements left in production code
- [ ] All components that use hooks or browser APIs have `"use client"` directive
- [ ] Import paths use project aliases (`@/components/`, `@/lib/`)
- [ ] No hardcoded colors — all colors reference theme tokens
- [ ] No inline styles where Tailwind classes suffice

---

## Content & Assets

- [ ] All placeholder content replaced with actual content (or clearly marked as placeholder)
- [ ] No broken image links
- [ ] All external URLs are valid and accessible
- [ ] Logo images are crisp (SVG preferred, or 2x resolution PNG)
- [ ] Video sources load without CORS issues
- [ ] Font files are loaded (Google Fonts or local) and applied correctly
