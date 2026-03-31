# Component Integration Guide

## 21st.dev Magic MCP Tools

### Browsing Inspiration
Use `mcp__magic__21st_magic_component_inspiration` to search for component types:
- Hero sections, testimonial carousels, animated tabs
- Card grids, sliders, pricing tables, feature sections
- Navigation bars, footers, CTA blocks

Provide descriptive queries: "animated tabs with sliding indicator", "testimonial carousel with 3D effect", "hero section with video background".

### Building Components
Use `mcp__magic__21st_magic_component_builder` to generate components:
- Provide brand colors and style preferences in the prompt
- Specify the framework (React, Next.js) and styling (Tailwind CSS)
- Request specific animations or interactions
- Include responsive behavior requirements

### Refining Components
Use `mcp__magic__21st_magic_component_refiner` to polish:
- Adjust spacing, colors, and animations to match brand tokens
- Fix responsive issues
- Add missing accessibility attributes
- Optimize animation performance

### Logo Search
Use `mcp__magic__logo_search` to find company/brand logos for partner sections.

---

## Adapting MCP Components for Your Project

### Token Replacement
MCP-generated components often use generic shadcn tokens. Replace with project-specific tokens:

```
text-muted-foreground  →  text-body
text-foreground        →  text-heading
bg-secondary           →  bg-bg-light
bg-primary             →  bg-primary (keep if defined in theme)
border-border          →  border-border (keep if defined in theme)
```

### Required Adaptations
1. Add `"use client"` directive for components using hooks or browser APIs
2. Ensure imports match project path aliases (`@/components/ui/`, `@/lib/utils`)
3. Replace any hardcoded colors with CSS custom properties or Tailwind tokens
4. Test with `next build` after each integration
5. Verify the component works at all breakpoints (375px, 768px, 1024px, 1440px)

### Common Issues
- **Missing dependencies**: Check if the component needs packages not yet installed (e.g., `framer-motion`, `class-variance-authority`)
- **Conflicting styles**: MCP components may include inline styles that override Tailwind classes — remove inline styles in favor of utility classes
- **Animation imports**: Some components import from `framer-motion`, others from `motion/react` — standardize to whichever the project uses

---

## Framer Motion Integration

### Installation
```bash
npm install framer-motion
# or
npm install motion
```

Import from `framer-motion` or `motion/react` depending on version:
```tsx
import { motion, AnimatePresence } from "framer-motion";
// or
import { motion, AnimatePresence } from "motion/react";
```

### Scroll-Triggered Entrance
```tsx
<motion.div
  initial={{ opacity: 0, y: 30 }}
  whileInView={{ opacity: 1, y: 0 }}
  viewport={{ once: true }}
  transition={{ delay: index * 0.1, duration: 0.5 }}
>
  {/* content */}
</motion.div>
```

### Staggered Children
```tsx
const containerVariants = {
  hidden: {},
  visible: {
    transition: { staggerChildren: 0.1 }
  }
};

const itemVariants = {
  hidden: { opacity: 0, y: 20 },
  visible: { opacity: 1, y: 0 }
};

<motion.div variants={containerVariants} initial="hidden" whileInView="visible" viewport={{ once: true }}>
  {items.map((item) => (
    <motion.div key={item.id} variants={itemVariants}>
      {/* content */}
    </motion.div>
  ))}
</motion.div>
```

### Content Swap with AnimatePresence
```tsx
<AnimatePresence mode="wait">
  <motion.div
    key={activeKey}
    initial={{ opacity: 0, y: 10 }}
    animate={{ opacity: 1, y: 0 }}
    exit={{ opacity: 0, y: -10 }}
    transition={{ duration: 0.2 }}
  >
    {/* content changes based on activeKey */}
  </motion.div>
</AnimatePresence>
```

### Word-by-Word Blur Reveal
```tsx
<motion.p>
  {text.split(" ").map((word, i) => (
    <motion.span
      key={i}
      initial={{ filter: "blur(10px)", opacity: 0, y: 5 }}
      animate={{ filter: "blur(0px)", opacity: 1, y: 0 }}
      transition={{ duration: 0.22, ease: "easeInOut", delay: 0.025 * i }}
      style={{ display: "inline-block" }}
    >
      {word}&nbsp;
    </motion.span>
  ))}
</motion.p>
```

### Hover & Tap Interactions
```tsx
<motion.button
  whileHover={{ scale: 1.05, y: -2 }}
  whileTap={{ scale: 0.97 }}
>
  Click me
</motion.button>
```

**Anti-pattern**: Avoid `whileHover: { scale }` on cards or large elements — it shifts layout. Use CSS `transform` with `transition` instead for cards.

---

## Key UI Components

### AnimatedTabs (Clip-Path Sliding)
A tab component with a sliding indicator using `clip-path` for smooth text color transitions:
- Container with relative positioning and brand-colored overlay
- Two text layers: base (inactive color) and clipped (active color)
- `clip-path: inset()` animated via `motion.div` for smooth sliding
- Support controlled mode with `activeTab` prop and `onTabChange` callback
- Add `whitespace-nowrap` to prevent text wrapping in tab labels

### InfiniteSlider + ProgressiveBlur
For logo carousels and continuous scrolling content:
- `InfiniteSlider`: Continuous scroll animation, configurable speed and direction
- `ProgressiveBlur`: Gradient fade on edges for seamless loop appearance
- Combine for partner logo sections, testimonial tickers, or announcement bars

### Circular Testimonials
3D carousel with perspective transforms:
- Show 3 images: center (active), left (rotated +15deg, scaled 0.85), right (rotated -15deg, scaled 0.85)
- `perspective: 1000px` on container for 3D depth
- Keyboard navigation (ArrowLeft/ArrowRight)
- Autoplay with 5-second interval, pauses on manual navigation
- Word-by-word blur reveal for quote text

---

## shadcn/ui Integration

### Adding a Brand Button Variant
In the Button component (`components/ui/button.tsx`), add:
```tsx
brand: "bg-brand-gradient text-white border-0 cursor-pointer",
```

### Common shadcn Components Used in Redesigns
- **Button**: With brand variant for CTAs
- **Badge**: For labels, tags, category indicators
- **Card**: Base structure for content cards (often customized heavily)
- **Tabs**: Can be replaced with AnimatedTabs for premium feel
- **Dialog/Sheet**: For mobile navigation menus

### Style Consistency
- Always add `cursor-pointer` to interactive elements
- Use project shadow tokens (`shadow-card`, `shadow-elevated`) instead of Tailwind defaults
- Maintain uniform `rounded-lg` (or chosen radius) across all card/image containers
