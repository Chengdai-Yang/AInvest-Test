# Ainvest Design System

Local design system for the Ainvest financial product. Authoritative source for tokens, rules, components, and page-generation skills.

---

## Repository Structure

```
ainvest-design-system/
├── SKILL.md                              # Main entry point — read first
├── README.md                             # This file
│
├── assets/
│   ├── tokens/
│   │   └── color.json                    # ⭐ Source of truth for ALL color values (hex / rgba)
│   ├── avatars/politicians/              # Local politician avatar PNGs
│   ├── icons/
│   └── logos/
│
├── references/
│   ├── tokens/                           # Token rules (no values — values are in assets/tokens/)
│   │   ├── color.md                      # Color token names, categories, usage rules
│   │   ├── typography.md                 # Font scale, weights, line heights
│   │   ├── spacing.md                    # 8px base unit scale
│   │   ├── radius.md                     # Corner radius tokens
│   │   ├── shadow.md                     # Elevation tokens
│   │   └── stroke.md                     # Border width + color tokens
│   │
│   ├── rules/                            # Non-token visual and interaction rules
│   │   ├── layout-grid.md                # Breakpoints, grid, fixed/adaptive width
│   │   ├── component-selection.md        # Ainvest-specific component choice rules
│   │   ├── card.md                       # Card style + interaction + carousel rules
│   │   ├── color-opacity-on-overlap.md   # Floating element solid-fill rule
│   │   ├── list-table-pattern.md         # List/table structure + alignment
│   │   ├── image-asset-usage.md          # Image shape + CDN URL patterns
│   │   └── perceptual-ux-heuristics.md   # Visual hierarchy, proximity, contrast heuristics
│   │
│   └── components/                       # Component-specific docs
│       └── button.md
│
└── skills/                               # Page-generation workflows
    └── page-spec-writer.md               # Workflow for writing Page Specs
```

---

## How to Use

### Generating a UI page

1. Read `SKILL.md` — it is the entry point
2. Follow the read order defined in SKILL.md §3 (Generation Workflow)
3. Load only the token / rule / component files the current page requires
4. Read color values from `assets/tokens/color.json` — never from `.md`

### Writing a Page Spec

1. Read `skills/page-spec-writer.md`
2. Follow its Pre-Design Protocol (P1–P4) and 7-section structure
3. Output to the project's `temp/specs/` directory with filename `{page-name}-spec.md`

---

## Source of Truth Rules

| Content type              | Source of truth                                       |
|---------------------------|-------------------------------------------------------|
| Color values (hex / rgba) | `assets/tokens/color.json` (JSON — machine-readable)  |
| Color token names / rules | `references/tokens/color.md`                          |
| Typography, spacing, radius, shadow, stroke | Corresponding `references/tokens/*.md` files |
| Layout / grid / image / list / card rules   | `references/rules/*.md`                       |
| Component behavior                          | `references/components/*.md`                  |
| Page Spec writing workflow                  | `skills/page-spec-writer.md`                  |

---

## Rules

- NEVER write raw hex / rgba values in `.md` files or generated code
- NEVER invent new tokens (colors, font sizes, weights, spacing, radius, shadow)
- When a component-level rule exists, it overrides the global token rule
- When this system defines a rule, follow it — do not apply generic habits

See `SKILL.md §4 Global Constraints` for the complete list.
