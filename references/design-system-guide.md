# Design System Setup Guide

## Generating Recommendations

Before defining tokens, use the UI/UX Pro Max skill to get data-driven recommendations:

```bash
# Generate full design system recommendation
python3 <ui-ux-pro-max-path>/scripts/search.py "<industry> <keywords>" --design-system -p "Project Name"

# Example queries by industry
python3 scripts/search.py "education edtech courses" --design-system -p "Academy"
python3 scripts/search.py "saas dashboard analytics" --design-system -p "Platform"
python3 scripts/search.py "ecommerce fashion luxury" --design-system -p "Store"
python3 scripts/search.py "healthcare medical portal" --design-system -p "Health"
```

The output includes: recommended pattern, style, color palette, typography pairing, key effects, and anti-patterns to avoid.

## Tailwind v4 Token Setup

For projects using Tailwind CSS v4 with the `@theme inline` syntax:

```css
/* globals.css or app.css */
@import "tailwindcss";

@theme inline {
  /* Colors — derived from design system recommendation */
  --color-primary: #BRAND_PRIMARY;
  --color-primary-dark: #BRAND_DARK;
  --color-heading: #TEXT_DARK;
  --color-body: #TEXT_MEDIUM;
  --color-gray-medium: #TEXT_LIGHT;
  --color-border: #E5E7EB;
  --color-bg-light: rgba(17, 25, 39, 0.04);
  --color-white: #FFFFFF;

  /* Typography */
  --font-sans: "Open Sans", sans-serif;

  /* Shadows */
  --shadow-card: 0px 1px 5px rgba(0, 0, 0, 0.08);
  --shadow-elevated: 0px 5px 22px rgba(0, 0, 0, 0.08);
}
```

### Usage in Components
```html
<h1 class="text-heading">...</h1>
<p class="text-body">...</p>
<div class="bg-primary">...</div>
<div class="border-border">...</div>
<div class="shadow-card hover:shadow-elevated">...</div>
```

## Tailwind v3 Token Setup

For projects using traditional `tailwind.config.ts`:

```ts
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        primary: '#BRAND_PRIMARY',
        'primary-dark': '#BRAND_DARK',
        heading: '#TEXT_DARK',
        body: '#TEXT_MEDIUM',
        border: '#E5E7EB',
        'bg-light': 'rgba(17, 25, 39, 0.04)',
      },
      boxShadow: {
        card: '0px 1px 5px rgba(0, 0, 0, 0.08)',
        elevated: '0px 5px 22px rgba(0, 0, 0, 0.08)',
      },
    },
  },
}
```

## CSS Utility Classes

Add these reusable utility classes to your global stylesheet:

```css
/* Brand button — solid color with refined hover states */
.bg-brand-gradient {
  background: var(--color-primary);
  transition: background 200ms ease, box-shadow 200ms ease, transform 150ms ease;
}
.bg-brand-gradient:hover {
  background: var(--color-primary-dark);
  box-shadow: 0 4px 14px rgba(var(--color-primary-rgb), 0.35);
}
.bg-brand-gradient:active {
  background: color-mix(in srgb, var(--color-primary-dark) 85%, black);
  transform: scale(0.98);
}

/* Decorative text gradient — for headings only, never buttons */
.text-brand-gradient {
  background: linear-gradient(135deg, var(--color-primary) 0%, color-mix(in srgb, var(--color-primary) 70%, white) 50%, var(--color-primary-dark) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}
```

## Button Design Rules

1. **CTA buttons**: Solid brand color background, white text
2. **Hover**: Darken primary by ~10%, add glow shadow
3. **Active/Pressed**: Darken by ~15%, `scale(0.98)`
4. **Outline buttons**: `border-border` with brand color text, fill on hover
5. **Ghost buttons**: No background, brand color text, subtle bg on hover
6. **Never**: Use amber-to-brown gradients or any muddy color transitions on buttons
7. **Always**: `cursor-pointer` on all interactive elements

### shadcn Button Variant
Add a `brand` variant to the shadcn Button component:
```tsx
brand: "bg-brand-gradient text-white border-0 cursor-pointer",
```

## Spacing Scale

Maintain a consistent 4px-based spacing rhythm:
- `gap-2` (8px) — tight spacing within cards
- `gap-4` (16px) — standard component spacing
- `gap-5` (20px) — grid gaps between cards
- `gap-6` (24px) — between nav items
- `gap-8` (32px) — between content blocks
- Section padding: `py-14 md:py-20` or `py-16 md:py-24`

## Typography Scale

```
Heading 1: text-5xl md:text-6xl xl:text-7xl font-extrabold tracking-tight
Heading 2: text-[2rem] md:text-[2.5rem] lg:text-[3rem] font-bold
Heading 3: text-xl font-bold
Body: text-base md:text-lg leading-relaxed
Small: text-sm
Label: text-xs font-bold uppercase tracking-widest
```
