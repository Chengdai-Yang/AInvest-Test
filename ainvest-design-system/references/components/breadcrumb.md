# Guidelines — Breadcrumb

> **Component Spec:** `ainvest-design-system/references/components/breadcrumb.md`

---

## 0. Document Role

This document covers **when and how to use** the Breadcrumb (面包屑) component. It focuses on decisions a designer must make explicitly — visual details handled automatically by the component are omitted.

- Component import: `@ainvest/breadcrumb`

---

## 1. Definition

Breadcrumb is a **secondary navigation component** that displays the user's current location within the site's information architecture and provides a path to navigate back up the hierarchy.

It supplements — not replaces — primary navigation (e.g. top nav bar, sidebar).

---

## 2. When to Use

| Condition | Use Breadcrumb? |
|-----------|-----------------|
| Page has a clear hierarchical position (e.g. Markets > USA > Stocks > Electronic Technology) | YES |
| Page is deeper than 2 levels in the information architecture | YES |
| User may need to navigate back to a parent category | YES |
| Page content is dynamically generated from a hierarchical path (e.g. search results, filtered lists) | YES |
| User arrived via search or filter and needs to see the navigation context | YES |

### MUST

- MUST use Breadcrumb when the page is at level 3 or deeper in the site hierarchy
- MUST use Breadcrumb when the page is part of a drill-down flow (e.g. Markets → USA → Stocks → Sector)

### SHOULD

- SHOULD use Breadcrumb on level 2 pages if the parent context aids orientation

---

## 3. When NOT to Use

| Condition | Use Breadcrumb? | Alternative |
|-----------|-----------------|-------------|
| Page is a top-level / landing page (L1) | NO | Primary nav already indicates location |
| Page is a standalone feature with no hierarchy (e.g. SPA tools like TradingView Options) | NO | None needed |
| Page structure is flat (all content at one level) | NO | Tab or primary nav |
| Page is a modal or overlay | NO | Modal header / close action |
| Mobile app with native back navigation | NO | System back gesture / button |

### MUST NOT

- MUST NOT use Breadcrumb as primary navigation — it is supplementary
- MUST NOT use Breadcrumb on pages that have no parent-child relationship
- MUST NOT use Breadcrumb in modals, drawers, or floating panels

---

## 4. Anatomy

```
[ Node 1 ]  /  [ Node 2 ]  /  [ ... ]  /  [ Node N-1 ]  /  [ Current Node ]
  (link)    sep   (link)    sep (overflow) sep  (link)    sep   (plain text)
```

| Part | Required | Description |
|------|----------|-------------|
| Root node (first node) | MUST | The top-level entry point (e.g. "Markets") |
| Intermediate nodes | MUST (if depth > 2) | Parent categories between root and current page |
| Current node (last node) | MUST | The current page — displayed as plain text, NOT clickable |
| Separator | MUST | Visual divider between nodes — use `/` or `>` per system convention |
| Overflow indicator (`...`) | Conditional | Appears when breadcrumb is truncated due to space constraints |
| Overflow dropdown | Conditional | Shown on hover of `...`, lists collapsed intermediate nodes |

### Rules

- MUST always display at minimum: root node + current node
- MUST NOT make the current (last) node clickable — it represents the active page
- MUST display all nodes as clickable links except the current node
- Overflow indicator MUST be interactive — hover reveals a dropdown listing the collapsed nodes

---

## 5. Variants Overview

| Variant | Figma key | Usage scenario |
|---------|-----------|----------------|
| Full breadcrumb | `@reuse@ainvest/breadcrumb` | Default — all nodes visible when space permits |
| Collapsed breadcrumb | Single element: `Title  Title  ...` | When horizontal space is insufficient — intermediate nodes collapse into `...` |

### Decision Logic

```
if available_width >= total_breadcrumb_width:
    use Full breadcrumb (all nodes visible)
else:
    use Collapsed breadcrumb:
        MUST keep: root node + current node
        MUST collapse: intermediate nodes (starting from level 2)
        MUST show: "..." as overflow indicator with hover dropdown
```

---

## 6. Content Guidelines

### Node Label

- MUST use the actual page/category name — MUST NOT abbreviate arbitrarily
- MUST keep labels concise — avoid labels longer than ~20 characters when possible
- SHOULD support hover tooltip showing the full label if truncated
- MUST use sentence case or title case consistent with the page title

### Separator

- Use the system-defined separator character (typically `/`)
- MUST NOT use custom decorative separators

### Overflow (`...`)

- MUST appear as a single `...` node when intermediate levels are collapsed
- MUST show a dropdown on hover listing collapsed nodes
- Dropdown items MUST be clickable — each navigates to the corresponding level page
- Root node and current node MUST NOT be collapsed into `...`

---

## 7. Layout & Composition

### Position

- MUST be placed below the global navigation bar
- MUST be placed above the page title / main content area
- MUST be left-aligned with the content area

### Relationship with Other Components

| Component | Relationship |
|-----------|-------------|
| Top navigation bar | Breadcrumb sits below nav; MUST NOT overlap or duplicate nav items |
| Page title | Breadcrumb sits above page title; the last breadcrumb node and page title should represent the same page |
| Tab (page-level) | Breadcrumb sits above tabs if both are present |
| Search bar (global) | Independent — breadcrumb remains visible during search |

### Spacing

- Follow `ainvest-design-system/references/spacing.md` for vertical gap between breadcrumb and adjacent elements
- Breadcrumb row MUST NOT introduce extra card or container — render directly on the page surface

---

## 8. Behavior

### Color States

| Element | State | Token |
|---------|-------|-------|
| Link node (parent levels) | Default | `color.text.primary` |
| Link node (parent levels) | Hover | `color.text.secondary` |
| Separator (`/`) | — | `color.text.quaternary` |
| Current node (last) | — | `color.text.quaternary` |
| Overflow trigger (`...`) | Default | `color.text.primary` |
| Overflow trigger (`...`) | Hover | `color.text.secondary` |

- Link nodes MUST use `color.text.primary` by default — MUST NOT use `color.text.secondary` as the resting state
- Hover MUST dim the node to `color.text.secondary` — NOT brighten it
- Current (last) node MUST use `color.text.quaternary` — MUST NOT use `color.text.tertiary`

### Click Behavior

- Clicking any node (except the current/last node) MUST navigate to that node's corresponding page
- MUST NOT open links in a new tab by default — use same-window navigation
- The current (last) node MUST NOT respond to click — MUST visually appear as non-interactive (use `color.text.quaternary`)

### Overflow Behavior

- When the breadcrumb exceeds available width, MUST collapse intermediate nodes into `...`
- Collapse priority:
  1. MUST keep the root node (first) and current node (last) visible at all times
  2. MUST collapse from the second-level nodes first (closest to root)
  3. On hover of `...`, MUST show a dropdown listing all collapsed nodes
  4. Clicking a dropdown item MUST navigate to that level's page

### Keyboard Behavior

- Each clickable node MUST be focusable via Tab key
- Enter / Space on a focused node MUST trigger navigation
- Overflow dropdown MUST be accessible via keyboard

---

## 9. Platform Differences

### Web

| Breakpoint | Behavior |
|------------|----------|
| width > 1410px | Full breadcrumb; width follows content panel |
| 990px < width <= 1410px | Full breadcrumb; width follows content panel |
| 500px < width <= 990px | Collapsed breadcrumb likely; intermediate nodes collapse to `...` |
| width <= 500px | Collapsed breadcrumb; width follows bottom sheet or card panel |

- On all Web breakpoints, breadcrumb row width MUST follow the content panel width (not viewport width)
- MUST NOT fix breadcrumb width independently of the content layout

### App (Mobile)

- MUST ensure breadcrumb node tap targets meet minimum hot zone requirements (minimum 44px height for touch)
- SHOULD evaluate whether native back navigation makes breadcrumb redundant
- If breadcrumb is used on mobile, MUST collapse aggressively — show at most: root + `...` + current

---

## 10. Do / Don't

| | Practice | Reason |
|---|---------|--------|
| DO | Show full path when space permits | Users can scan the complete hierarchy at a glance |
| DO | Collapse intermediate nodes with `...` when space is tight | Preserves root + current context without horizontal overflow |
| DO | Make `...` hoverable to reveal collapsed nodes in a dropdown | Users can still access any level without extra navigation |
| DO | Keep the last node as plain non-interactive text | Avoids confusion — clicking the current page is meaningless |
| DON'T | Use breadcrumb on top-level landing pages | No parent hierarchy to show — adds visual noise |
| DON'T | Use breadcrumb in SPA tools with flat structure | Misleading — implies hierarchy that doesn't exist |
| DON'T | Make the current (last) node clickable | Violates breadcrumb convention; creates a no-op click |
| DON'T | Truncate node labels silently without hover tooltip | Users lose context if labels are cut without explanation |
| DON'T | Collapse the root or current node into `...` | These are the two essential orientation anchors |
| DON'T | Use breadcrumb inside modals or floating panels | Breadcrumb is a page-level construct, not a panel-level one |

---

## 11. Decision Table

| Page depth | Page type | Hierarchy clear? | Platform | Action |
|------------|-----------|-------------------|----------|--------|
| L1 (top-level) | Landing / Home | — | Any | MUST NOT use breadcrumb |
| L2 | Category page | YES | Web | SHOULD use breadcrumb |
| L3+ | Detail / sub-category | YES | Web | MUST use breadcrumb |
| Any | Standalone SPA tool | NO hierarchy | Web | MUST NOT use breadcrumb |
| Any | Modal / Drawer | — | Any | MUST NOT use breadcrumb |
| L3+ | Detail page | YES | App | Evaluate: if native back nav is sufficient → SHOULD NOT; otherwise → MAY use collapsed breadcrumb |

### Special Scenarios

| Scenario | Breadcrumb behavior |
|----------|---------------------|
| User arrives via search/filter to a category page | MUST show full hierarchical path; breadcrumb may also reflect the search context (e.g. filter conditions) |
| Dynamically generated list page (e.g. "Pre-market most active") | MUST show the path from root to the current dynamic list; nodes are generated from the navigation path |
| Page with both breadcrumb and page-level tabs | Breadcrumb sits above tabs; both are visible simultaneously |

---

## 12. Related Documents

- `ainvest-design-system/references/components/breadcrumb.md` — Component style spec (组件样式定义、API、代码示例)
- `ainvest-design-system/SKILL.md` — Unified skill file
- `ainvest-design-system/references/spacing.md` — Spacing tokens for layout
- `ainvest-design-system/references/typography.md` — Text styles for node labels
- `ainvest-design-system/references/color.md` — Color tokens for link / muted / hover states
- `ainvest-design-system/references/patterns/page-structure-types.md` — L1 / L2 / L3 page classification
