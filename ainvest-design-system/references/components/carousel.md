# Guidelines — carousel

> **Component Spec:** `ainvest-design-system/references/components/carousel.md`

---

## 0. Document Role

This document covers **when and how to use** the carousel (轮播图) component. It focuses on decisions a designer must make explicitly — visual details handled automatically by the component are omitted.

- Component import: `@ainvest/carousel`

This document focuses on the **Web Carousel Indicator** sub-system (Dot Indicator / Free-Scroll Indicator / Pagination Arrow). Visual tokens and anatomy of the three indicator sub-components are detailed in §3 onwards.

---

## 1. Definition

The Carousel (轮播图) is a component that displays multiple cards or images within a fixed area, supporting automatic or manual cycling. It is used for homepage Banners, promotional recommendations, and product showcases.

The **Web Carousel Indicator** system consists of three sub-components:

| Sub-Component | Purpose |
|---------------|---------|
| **Dot Indicator** | Shows total pages and current position via dots; supports full-page snapping |
| **Free-Scroll Indicator** | Progress bar for free-scroll (non-snapping) content; proportional width indicates visible portion |
| **Pagination Arrow** | Left/right arrow buttons for manual page navigation; 3 size tiers |

---

## 2. When to Use

| Condition | Use? | Sub-Component |
|-----------|------|---------------|
| Full-page carousel (Banner, hero, card deck) with snap-to-page | YES | Dot Indicator + Pagination Arrow |
| Card grid that supports horizontal free-scroll (no snap) | YES | Free-Scroll Indicator |
| Single-row horizontal card list with fixed page count | YES | Dot Indicator + Pagination Arrow |
| Onboarding / walkthrough pages | YES | Dot Indicator |
| Content has only 1 page (no overflow) | NO | No indicator needed |
| Vertical scrollable content | NO | Use Scroll Bar instead |
| Paginated data table | NO | Use Pagination component |

---

## 3. When NOT to Use

| Condition | Use Instead |
|-----------|------------|
| Vertical scroll | Scroll Bar |
| Data table pagination (page numbers) | Pagination component |
| Single image / no overflow | No indicator |
| Tab-based content switching | Tab component |
| Mobile-only swipe carousel | Mobile carousel indicator (separate Figma component) |

---

## 4. Anatomy

### 4a. Dot Indicator

```
              ● ○ ○        ← Dots centered below/inside card
```

- **Active dot**: Solid circle, `#000000` (light) / `#FFFFFF` (dark)
- **Inactive dot**: Same shape, reduced opacity — `rgba(0,0,0,0.10)` on light cards, `rgba(255,255,255,0.12)` on dark cards
- Dots are horizontally centered relative to the card/carousel area

### 4b. Free-Scroll Indicator

```
         ▓▓▓░░░░░░░░        ← Progress bar below cards
```

- **Track**: Full-width bar (total length fixed)
- **Active segment**: Proportional width = (visible columns / total columns) × total length
- Active segment fills dark (`#000000` / `#FFFFFF`)
- Track remainder is subtle divider color

### 4c. Pagination Arrow

```
    ┌───┐                           ┌───┐
    │ ‹ │    [   CARD CONTENT   ]   │ › │
    └───┘                           └───┘
```

- Circular buttons with chevron icon
- 3 size tiers (see §5)
- Can be positioned inside or outside the card area

---

## 5. Variants Overview

### 5a. Dot Indicator Sizes

| Breakpoint | Dot Size | Spacing (top/bottom) | Position |
|------------|---------|---------------------|----------|
| Web > 500px | 8px diameter | 16px top/bottom | Centered below or inside card |
| Web ≤ 500px | 6px diameter | 8px top/bottom | Centered below or inside card |
| App (Mobile) | 6px diameter | 8px top/bottom | Centered below card |

### 5b. Free-Scroll Indicator Sizes

| Platform | Bar Height | Total Length | Active Segment | Min Active | Max Active |
|----------|-----------|-------------|---------------|-----------|-----------|
| Web | 6px | 36px | `(visibleCols / totalCols) × 36` | 6px | 20px |
| App | 3px | 24px | `(visibleCols / totalCols) × 24` | 6px | 20px |

### 5c. Pagination Arrow Tiers

| Tier | Button Size | Icon Size | Hit Area | Condition | Usage |
|------|------------|----------|----------|-----------|-------|
| **Tier 1** | 48px circle | 24px | 48×48px | Component height > 44px | Major modules — chart, index cards, hero banners |
| **Tier 2** | 32px circle | 20px | 36×36px | Component height ≤ 44px | Page-level lightweight nav (top tab area) |
| **Tier 3** | 20–48px variable | 20px | 20×20px min | Component height ≤ 44px | Module title overflow hint (hidden until needed) |

---

## 6. Content Guidelines

### Dot Indicator
- No text content — purely visual

### Pagination Arrow
- Icon only — no text labels
- Chevron direction MUST match navigation direction (‹ = back, › = forward)

### MUST
- Active dot MUST be visually distinct (solid vs semi-transparent)
- Free-scroll indicator active segment MUST be proportional to visible/total ratio

### MUST NOT
- MUST NOT add page numbers to dots
- MUST NOT add text labels to arrows

---

## 7. Layout & Composition

### Dot Indicator Positioning

| Position | When | Spacing from bottom edge |
|----------|------|------------------------|
| **Inside card** (overlay) | When dots sit within the card visual area | 16px from card bottom edge |
| **Outside card** (below) | When dots sit below the card | 16px gap between card bottom and dots |

### Pagination Arrow Positioning

| Tier | Position | Offset from card/content edge |
|------|----------|------------------------------|
| Tier 1 (inside card) | Left/right edges inside the card | 24px from card inner edge |
| Tier 2 (outside card) | At Grouptitle row, right-aligned | Aligned with section title row |
| Tier 3 (inline) | Within module title bar | Adjacent to title content |

### Responsive Rules

| Breakpoint | Arrows (Tier 1) | Dots |
|------------|----------------|------|
| Web > 500px | Inside card, 24px margin from content edge | 8px dots, 16px spacing |
| Web ≤ 500px | Inside card, 8px margin from edge (tighter) | 6px dots, 8px spacing |

---

## 8. Behavior

### 8a. Dot Indicator Behavior

| Event | Behavior |
|-------|----------|
| Page changes (swipe or arrow click) | Active dot switches instantly — no transition animation on dots themselves |
| Auto-play (if enabled) | Dots update in sync with auto-advancing content |
| Dot click (optional) | Jump directly to corresponding page |

### 8b. Pagination Arrow Behavior

**Right Arrow (→):**

| State | Visibility |
|-------|-----------|
| First page, has more pages | Visible (default) |
| Middle pages | Visible |
| Last page | **Hidden immediately** — no fade, no delay |
| User scrolls back from last page | **Reappears immediately** |

**Left Arrow (←):**

| State | Visibility |
|-------|-----------|
| First page | **Hidden** |
| Second page or later | **Visible immediately** — no animation/slide-in |
| User returns to first page | **Hidden immediately** |

**Hover behavior:**
- On hover: button switches to highlight style (e.g. shadow deepens or background color changes)
- NO opacity fade or translateX animation on hover — instant style change
- Reference: Apple official interaction style (direct, immediate response)

**Disabled state:**
- Reduced opacity for arrows that cannot navigate further
- Only applies to Tier 1/2 when explicitly locked; normally arrows hide instead of disable

### 8c. Card/Banner Transition Animation

| Property | Value |
|----------|-------|
| Transition method | Horizontal slide (`translateX()`) |
| Duration | 300ms |
| Easing | `cubic-bezier(0.4, 0, 0.2, 1)` (Material ease) |
| Trigger | One click on arrow = one page transition |

**Click right arrow (→):**
- Current card: `translateX(0)` → `translateX(-100%)`
- Next card: `translateX(100%)` → `translateX(0)`

**Click left arrow (←):**
- Current card: `translateX(0)` → `translateX(100%)`
- Previous card: `translateX(-100%)` → `translateX(0)`

### 8d. Pagination Arrow Color States

**Light background (浅色):**

| State | Style |
|-------|-------|
| Default | White fill + border stroke |
| Hover | Shadow deepens (投影加深) |
| Disabled | Reduced opacity |

**Dark background (深色):**

| State | Style |
|-------|-------|
| Default | Black fill, white icon |
| Hover | Background color lightens |
| Disabled | Reduced opacity |

### 8e. Free-Scroll Indicator Behavior

- Active segment position tracks scroll position proportionally
- Moves continuously as user scrolls (not step-based)
- No snap behavior — indicator stops where user stops scrolling

---

## 9. Platform Differences

| Aspect | Web (>500px) | Web (≤500px) | App (Mobile) |
|--------|-------------|-------------|--------------|
| Dot Indicator size | 8px | 6px | 6px |
| Dot spacing | 16px | 8px | 8px |
| Free-scroll bar | 6px × 36px total | 6px × 36px total | 3px × 24px total |
| Arrow Tier 1 | 48px, 24px margin | 48px, 8px margin | N/A (swipe gesture) |
| Arrow Tier 2 | 32px, at title row | 32px, at title row | N/A |
| Arrow hover | Yes (shadow/color) | Yes | N/A |
| Transition | translateX 300ms | translateX 300ms | Native swipe |

---

## 10. Do / Don't

### DO

| # | Rule |
|---|------|
| 1 | Use Dot Indicator for fixed-page-count carousels (Banner, card deck) |
| 2 | Use Free-Scroll Indicator for variable-length horizontal content (card grids) |
| 3 | Hide left arrow on first page, hide right arrow on last page — no fade animation |
| 4 | Use 300ms `cubic-bezier(0.4, 0, 0.2, 1)` for card slide transitions |
| 5 | Switch dot state instantly with page change (no dot transition animation) |
| 6 | Ensure active/inactive dots have clear contrast (solid vs 10-12% opacity) |
| 7 | Scale dots from 8px to 6px at ≤500px breakpoint |

### DON'T

| # | Rule |
|---|------|
| 1 | DON'T add fade/slide animation to arrow show/hide — instant visibility toggle |
| 2 | DON'T use opacity transitions on arrow hover — use direct color/shadow change |
| 3 | DON'T show pagination arrows when there is only 1 page |
| 4 | DON'T use Free-Scroll Indicator for snap-to-page carousels (use Dot Indicator) |
| 5 | DON'T mix Dot Indicator and Free-Scroll Indicator on the same carousel |
| 6 | DON'T keep disabled arrows visible when they should be hidden |
| 7 | DON'T use vertical pagination arrows (this component is horizontal-only) |

---

## 11. Decision Table

### Which indicator type?

| Content Type | Scroll Behavior | → Indicator |
|-------------|----------------|-------------|
| Banner / hero images | Snap to page | Dot Indicator |
| Card deck (fixed count per page) | Snap to page | Dot Indicator |
| Card grid (variable count, free scroll) | Free scroll, no snap | Free-Scroll Indicator |
| Onboarding flow | Snap to page | Dot Indicator |

### Which arrow tier?

| Component Height | Importance | → Arrow Tier |
|-----------------|-----------|-------------|
| > 44px | Primary content module | Tier 1 (48px, inside card) |
| ≤ 44px | Page-level nav element | Tier 2 (32px, outside card) |
| ≤ 44px | Module title overflow | Tier 3 (20px, inline with title) |

### Dot position?

| Condition | → Position |
|-----------|-----------|
| Carousel is a hero banner / full-bleed image | Inside card (16px from bottom) |
| Carousel shows discrete cards with gaps | Outside card (16px below card) |

---

## 12. Related Documents

| Document | Relationship |
|----------|-------------|
| `references/components/WebCarouselIndicator.md` | Dev spec (pending) |
| `references/guidelines/Pagination.md` | Data table pagination — separate component |
| `references/guidelines/Tab.md` | Tab switching — NOT carousel |
| `references/guidelines/ScrollBar.md` | Vertical/horizontal scroll — NOT carousel |
| `references/guidelines/Button.md` | Arrow buttons follow general button state rules |
