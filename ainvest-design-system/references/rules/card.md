# Card Rules

**Category:** Visual Style + Interaction Pattern
**Scope:** All card components — Comparison Card, Popular Comparison Card, News Card, Stock Card, Trade Event Card, Politician Card, any `border-radius` surface that acts as a contained content unit

---

## 1. Visual Style (Static)

### Background

- MUST use `color.bg.surface` (`#FFFFFF` in light mode) as card background
- MUST NOT use `color.bg.weak` or `color.bg.page` as card background

### Border

- MUST use a `1px solid` border with `color.divider.level3` (`rgba(0,0,0,0.05)`) as the default card outline
- MUST NOT use `shadow.md` or any box-shadow to define card boundaries — borders carry this role
- MUST NOT use `color.border.default` for the default state (too prominent; reserved for focus/selected states)

### Corner Radius

- Default card → `radius.md` (16px)
- Large card / floating panel → `radius.lg` (24px)
- MUST NOT mix radius values within the same card component

### Padding

- Default card → `spacing.16` (16px) internal padding
- Spacious card (e.g. form input surface, hero surface) → `spacing.24` (24px) internal padding
- MUST NOT use arbitrary padding values

### Internal Spacing

- Gap between elements within a card → `spacing.8` (8px)
- Gap between sub-sections within a card → `spacing.12` (12px)

### Shadow

- MUST NOT use any `box-shadow` on cards in default state
- Shadow is reserved for overlays, dropdowns, and modals — not cards
- Elevation is communicated by border + background contrast, not shadow

---

## 2. Hover State

### MUST

- MUST use a **flat background fill** as the sole hover feedback
- MUST use `background: #F5F5F5` on hover
- MUST apply `transition: background 0.15s` for smooth entry
- MUST keep border unchanged on hover

### MUST NOT

- MUST NOT use `transform: translateY(-Npx)` (vertical lift)
- MUST NOT add or increase `box-shadow` on hover
- MUST NOT use `opacity` changes on hover

### Rationale

Lift-and-shadow hover in dense financial interfaces causes layout shifts, over-signals importance, and competes with real elevation signals (modals, dropdowns). Flat fill communicates interactivity without visual noise.

---

## 3. Active / Pressed State

- MUST darken slightly from hover: `background: #EBEBEB`
- MUST keep border unchanged
- MUST NOT apply scale transform

---

## 4. Selected State (when applicable)

- MUST change border to `1px solid color.border.default` (`rgba(0,0,0,0.2)`) to indicate selection
- MUST NOT use shadow to communicate selection
- MAY add inset ring: `box-shadow: inset 0 0 0 2px rgba(0,0,0,0.2)` for stronger selection confirmation

---

## 5. State Reference Table

| State          | Background         | Border                          | Shadow | Transform |
|----------------|--------------------|---------------------------------|--------|-----------|
| Default        | `color.bg.surface` | 1px `color.divider.level3`      | none   | none      |
| Hover          | `#F5F5F5`          | 1px `color.divider.level3`      | none   | none      |
| Active/Pressed | `#EBEBEB`          | 1px `color.divider.level3`      | none   | none      |
| Selected       | `color.bg.surface` | 1px `color.border.default`      | none   | none      |

---

## 6. Carousel Rules

### Card Count

- MUST display **3–6 cards** in the visible area of a compact-card carousel (politician/stock cards)
- MUST display **3–4 cards** for wider event/trade cards
- MUST NOT display more than 6 compact cards at any breakpoint
- MUST NOT display fewer than 3 cards

Breakpoint targets for compact cards:

| Breakpoint    | Visible cards |
|---------------|---------------|
| XL ≥1410px    | 5             |
| LG 900–1410px | 4             |
| MD 500–900px  | 3             |

### Carousel Card Width

- MUST use `flex: 1 0 0` on carousel cards so they fill the full row width without gaps
- MUST retain `min-width` to prevent cards from becoming too narrow
- MUST NOT use fixed `width: calc(...)` formulas
- For MD breakpoint on event-track (wider cards), use `flex: 0 0 280px` to enable horizontal scroll

### Carousel Arrow Overlap

When carousel arrows overlap card borders, apply the **color-opacity-on-overlap** rule — arrows must use solid fill on hover, not transparent. See `rules/color-opacity-on-overlap.md`.

---

## 7. MUST / MUST NOT Summary

### MUST

- Use `color.bg.surface` as card background
- Use `1px solid color.divider.level3` as default border
- Use `radius.md` as default corner radius
- Use `spacing.16` as default internal padding
- Use flat fill `#F5F5F5` on hover
- Keep border unchanged across default / hover / active states

### MUST NOT

- Use `box-shadow` on cards (default, hover, or active)
- Use `transform: translateY(...)` on hover
- Use `opacity` changes on hover
- Use `color.border.default` as default border (too strong)
- Invent custom radius, padding, or border-color values

---

## 8. FAILURE

- If a card uses `box-shadow` in default state → remove shadow, add `1px solid color.divider.level3` border
- If a card hover adds or increases `box-shadow` → remove it, apply `background: #F5F5F5`
- If a card hover applies `transform: translateY(...)` → remove it
- If a card hover uses opacity → replace with flat fill
- If a card uses `radius.lg` for a standard-size card → change to `radius.md`
- If a compact card carousel shows >6 or <3 cards at any breakpoint → fix using `flex: 1 0 0`
- If a ticker logo inside a card appears square → see `rules/image-asset-usage.md`
