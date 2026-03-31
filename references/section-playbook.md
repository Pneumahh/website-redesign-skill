# Section Playbook

Detailed patterns for building each website section. All examples use Tailwind CSS utility classes and assume brand tokens are configured.

---

## Navigation / Header

### Scroll-Aware Navbar (3-State System)
The navbar should transition through 3 visual states based on scroll position:

| State | When | Style |
|-------|------|-------|
| Transparent | At top (scrollY < 50) | `bg-transparent`, white text, no shadow |
| Dark frosted glass | Over hero section | `bg-black/70 backdrop-blur-md`, white text |
| White solid | Past hero section | `bg-white shadow-card`, dark text |

**Implementation approach:**
1. Pass a `ref` from the hero section to the navbar component
2. On scroll, measure `heroRef.current.getBoundingClientRect().bottom`
3. Use the hero's actual bottom edge (not viewport percentage) for precise transition
4. Apply transitions: `transition-all duration-300`

**Why ref-based, not percentage-based:**
Using `window.innerHeight * 0.85` or similar is unreliable because the hero section height varies across breakpoints and may include sub-sections (partner logos, stats) that extend beyond the viewport.

### Mobile Navigation
- Hamburger icon on screens < 768px
- Slide-in panel from right with `AnimatePresence`
- Full-height overlay with blur backdrop
- Close on link click or backdrop tap

---

## Hero Section

### Layout Options
1. **Split layout**: Text left, image/video right (or reversed)
2. **Centered**: Full-width background with centered text overlay
3. **Video background**: Auto-playing muted video with text overlay

### Key Elements
- **Badge/pill**: Status indicator at top (e.g., "Now Enrolling", "New Feature")
  - Pulsing dot animation: `animate-ping` on a small circle
  - Rounded pill: `rounded-full bg-primary/10 border border-primary/20`
- **Headline**: `text-[2rem] md:text-[3rem] lg:text-[3.75rem] font-extrabold tracking-tight`
  - Use `.text-brand-gradient` on key words for emphasis
- **Subheading**: `text-lg text-body leading-relaxed max-w-xl`
- **CTA button**: `bg-brand-gradient` with arrow icon, `group-hover:translate-x-1` on arrow
- **Social proof strip**: Avatar stack + count + star rating

### Video Background
```tsx
<video autoPlay loop muted playsInline className="absolute inset-0 w-full h-full object-cover opacity-40">
  <source src="https://assets.mixkit.co/videos/{ID}/{ID}-720.mp4" type="video/mp4" />
</video>
```
- Keep opacity at 40-50% for readability
- Add gradient overlay: `bg-gradient-to-r from-black/80 via-black/50 to-transparent`
- Use dark text overlay for contrast

### Partner Logo Slider
- Place below hero content or as a sub-section
- Use `InfiniteSlider` with `ProgressiveBlur` edge fades
- Grayscale logos with hover color reveal: `grayscale hover:grayscale-0 transition`
- Label: `text-xs font-bold uppercase tracking-widest text-gray-medium`

---

## Two-Column Feature Section

### Structure
```
[Badge Label]
[Heading — text-[2rem] md:text-[2.5rem] font-bold]
[Description — text-body leading-relaxed]
[CTA Button with arrow →]
                                    [Image with hover zoom]
```

### Alternating Layout
Use a `reversed` prop to flip the column order:
```tsx
<div className={`flex flex-col ${reversed ? 'lg:flex-row-reverse' : 'lg:flex-row'} items-center gap-12`}>
```

### Image Treatment
- Container: `overflow-hidden rounded-lg shadow-card`
- Image: `transition-transform duration-700 ease-out group-hover:scale-110`
- Optional gradient overlay at bottom for text

### CTA Button
- Text + arrow icon
- Arrow translates right on hover: `group-hover:translate-x-1 transition-transform`
- Use `whileTap={{ scale: 0.97 }}` — avoid `whileHover: { scale }` (causes layout shift)

---

## Product/Service Grid with Category Tabs

### AnimatedTabs
- Clip-path sliding animation for active indicator
- Build tabs dynamically from data categories with counts: `"Category (N)"`
- "All" tab shows everything
- Add `whitespace-nowrap` to prevent label wrapping

### Card Grid
- Responsive: `grid-cols-1 md:grid-cols-2 lg:grid-cols-3`
- Gap: `gap-5` (20px)
- Cards filter with fade transition using `AnimatePresence`

### Card Design
```
[Image — h-44, object-cover, rounded-lg overflow-hidden]
  [Badge — absolute top-left, bg-primary/90 text-white text-xs]
[Content — px-4 pt-3.5 pb-4]
  [Title — font-bold text-heading]
  [Description — text-sm text-body line-clamp-2]
  [Info pills — rating ★, lesson count — appear on hover]
  [Price + CTA row — justify-between items-baseline]
```

### Hover Effects
- Card: `hover:-translate-y-1 hover:shadow-elevated transition-all duration-300`
- Image: `group-hover:scale-105 transition-transform duration-500`
- Info pills: `opacity-0 group-hover:opacity-100 transition-opacity`

---

## Testimonials

### Circular 3D Carousel
- Show 3 testimonials: center (active), left (rotated, scaled down), right (rotated, scaled down)
- Container: `perspective: 1000px`
- Active: `scale(1) rotateY(0deg)`
- Left: `translateX(-gap) scale(0.85) rotateY(15deg)`
- Right: `translateX(gap) scale(0.85) rotateY(-15deg)`
- Transition: `all 0.8s cubic-bezier(.4,2,.3,1)` for spring effect

### Quote Animation
- Word-by-word blur reveal using Framer Motion
- Each word: `initial={{ filter: "blur(10px)", opacity: 0 }}` → `animate={{ filter: "blur(0px)", opacity: 1 }}`
- Stagger: `delay: 0.025 * wordIndex`

### Navigation
- Arrow buttons with circular background
- Keyboard support: ArrowLeft/ArrowRight
- Autoplay: 5-second interval, pauses on manual navigation
- Dot indicators showing active testimonial

### Simple Card Carousel (Alternative)
- Horizontal scroll with snap: `overflow-x-auto snap-x snap-mandatory`
- Cards: `snap-center flex-shrink-0 w-80`
- Prev/Next buttons with disabled state at boundaries
- Dot indicators with active state animation

---

## Stats / Counter Section

### Layout
- Centered row of 3-4 stats
- Large number: `text-4xl md:text-5xl font-extrabold text-heading`
- Label: `text-sm text-body`
- Optional: animated count-up on scroll into view

### Count-Up Animation
```tsx
// Use whileInView to trigger count animation
<motion.span
  initial={{ opacity: 0 }}
  whileInView={{ opacity: 1 }}
  viewport={{ once: true }}
  onViewportEnter={() => startCounting()}
/>
```

---

## Community / Gallery Section

### Image Carousel
- Horizontal scroll with snap points
- Cards: `flex-shrink-0 w-72 md:w-80 snap-center`
- Image with gradient overlay and title at bottom
- Prev/Next arrow buttons + dot indicators
- Hide scrollbar: `scrollbar-width: none`

### Image Treatment
- `overflow-hidden rounded-lg`
- Hover: `group-hover:scale-110 transition-transform duration-500`
- Bottom gradient: `bg-gradient-to-t from-black/60 via-black/20 to-transparent`
- Title overlay: `absolute bottom-4 left-4 text-white font-semibold`

---

## Footer

### Structure
```
[Brand logo + tagline]
[Multi-column link groups — 3-4 columns]
  [Column heading — text-sm font-bold uppercase tracking-widest]
  [Links — text-sm text-body hover:text-primary transition]
[Contact info — email, phone, address]
[Social icons — Lucide icons in circular buttons]
[Divider]
[Copyright + legal links]
```

### Responsive
- Desktop: 4-5 columns in a row
- Tablet: 2x2 grid
- Mobile: single column, stacked

### Social Icons
- Use Lucide icons (not Font Awesome or emojis)
- Circular container: `w-10 h-10 rounded-full bg-bg-light hover:bg-primary hover:text-white transition`
- Consistent sizing: `h-5 w-5`
