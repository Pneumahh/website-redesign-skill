# Website Redesign Skill for Claude Code

A comprehensive, reusable Claude Code skill that guides end-to-end website redesigns from discovery to delivery. Works with any brand, any industry, any modern frontend stack.

## What It Does

This skill provides an 8-phase workflow for redesigning websites:

| Phase | Description |
|-------|-------------|
| 1. Discovery | Analyze the existing site, content, brand, and tech stack |
| 2. Design System | Generate color palettes, typography, spacing tokens via `/ui-ux-pro-max` |
| 3. Component Sourcing | Find and build premium UI components via 21st.dev Magic MCP |
| 4. Section Build | Build each page section with proven layout patterns |
| 5. Animation & Motion | Add scroll effects, hover states, and transitions with Framer Motion |
| 6. UX Polish | Audit accessibility, responsive behavior, and interaction quality |
| 7. Assets | Source images (Unsplash), videos (Mixkit), icons (Lucide) |
| 8. Review | Build verification, visual QA, and pre-delivery checklist |

## Installation

Copy this directory into your Claude Code skills folder:

```bash
cp -r website-redesign ~/.claude/skills/
```

## Usage

Invoke the skill in Claude Code:

```
/website-redesign
```

Or simply ask Claude Code to redesign a website — the skill triggers automatically on keywords like: redesign, revamp, rebuild UI, improve design, new homepage, landing page, website makeover, modernize website, refresh UI.

## Integrated Tools & Skills

| Tool/Skill | Purpose |
|------------|---------|
| `/ui-ux-pro-max` | Design system recommendations, UX guidelines, color palettes |
| `/frontend-design` | Visual quality review, typography, spatial composition |
| 21st.dev Magic MCP | Browse inspiration, generate components, refine designs |
| Framer Motion | Scroll animations, page transitions, micro-interactions |
| shadcn/ui | Base component library with custom brand variants |
| Tailwind CSS (v3/v4) | Utility-first styling with design tokens |

## File Structure

```
website-redesign/
├── SKILL.md                          # Core 8-phase workflow
├── README.md                         # This file
└── references/
    ├── design-system-guide.md        # Brand tokens, Tailwind setup, button rules
    ├── component-integration.md      # 21st.dev MCP, Framer Motion, shadcn patterns
    ├── section-playbook.md           # Hero, features, grids, testimonials, footer
    └── pre-delivery-checklist.md     # Visual, interaction, responsive, a11y audit
```

## Requirements

- [Claude Code](https://claude.ai/claude-code) CLI or desktop app
- A frontend project (Next.js, React, or similar)
- Tailwind CSS (v3 or v4)
- Optional: 21st.dev Magic MCP server configured
- Optional: `/ui-ux-pro-max` and `/frontend-design` skills installed

## Key Design Principles

- **Solid brand colors** for buttons — no muddy gradients
- **Uniform border-radius** across all image containers
- **Consistent spacing rhythm** on a 4px base scale
- **Scroll-aware navbar** with 3-state transitions (transparent/dark/white)
- **Performance-first animations** — `viewport={{ once: true }}`, no layout-shifting hovers
- **Accessibility by default** — 4.5:1 contrast, keyboard navigation, `prefers-reduced-motion`

## License

MIT
