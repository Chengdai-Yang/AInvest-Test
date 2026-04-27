# Guidelines — pill-tabs

> **Component Spec:** `ainvest-design-system/references/components/pill-tabs.md`

---

## 0. Document Role

This document covers **when and how to use** the pill-tabs (胶囊标签页) component. It focuses on decisions a designer must make explicitly — visual details handled automatically by the component are omitted.

- Component import: `@ainvest/pill-tabs`

---

## 1. Definition

pill-tabs is a **capsule-shaped tab** used for switching between sibling views that are **closely related** (typically different slices / filters of the same content domain, or a set of parallel modules inside one page).

The component exposes two levels via the `level` prop:

| `level` | Intended Scenario | Inactive Style |
|---------|-------------------|----------------|
| `"page"` | **Page-level secondary** switching — sub-sections within a page module | `secondary` (filled capsule on neutral background) |
| `"module"` | **Module-level primary** switching — top-most switcher inside a self-contained module | `outline` (bordered capsule) |

Compared with other tab sub-types:

| Sub-type | Purpose |
|----------|---------|
| line-tabs | Page-level primary navigation — independent sibling views |
| **pill-tabs** | Page-level secondary / module-level switching with capsule-shaped items |
| segment-tabs | Dense segmented control inside a bordered container (e.g. chart timeframe) |

---

## 2. When to Use

| Condition | Use pill-tabs? | Which `level` |
|-----------|----------------|---------------|
| Secondary switcher inside a page section (under line-tabs or a page title) | YES | `"page"` |
| Primary switcher at the top of a self-contained module (card / widget) | YES | `"module"` |
| Parallel slices of the same content domain (e.g. "Stocks / ETFs / Crypto" filters) | YES | `"page"` or `"module"` by hierarchy |
| A right-side action icon (e.g. filter, sort, settings) needs to live next to the tab row | YES | either (see §5b) |

### MUST

- MUST set `level` explicitly based on hierarchy — `"page"` for secondary, `"module"` for module-level primary
- MUST keep exactly one tab selected at all times (single-select)
- MUST keep the tab row on a single line (no wrapping)

### SHOULD

- SHOULD use pill-tabs when sibling tabs are **related slices** of one domain (if they are fully independent page sections, use line-tabs instead)
- SHOULD place pill-tabs directly above the content it switches, with no other interactive elements between them

---

## 3. When NOT to Use

| Condition | Use Instead |
|-----------|-------------|
| Page-level primary navigation between independent sections | line-tabs |
| Chart timeframe / dense segmented control inside a bordered container | segment-tabs |
| Action trigger (submit, save, delete) | Button |
| Navigation to a completely different page / URL | Link / Nav |
| Multi-select filtering | Checkbox group / Chip group |
| Only one panel exists (no switching needed) | Remove tabs entirely |

### MUST NOT

- MUST NOT use pill-tabs for actions — use Button
- MUST NOT use pill-tabs when only one panel is available (≥ 2 tabs required)
- MUST NOT allow zero tabs to be selected
- MUST NOT stack two pill-tabs rows at the same hierarchy level in one module

---

## 4. Anatomy

```
┌─ level="page" ─────────────────────────────────────┐
│  ╭───────╮ ╭───────╮ ╭───────╮ ╭───────╮           │
│  │ Tab A │ │ Tab B │ │ Tab C │ │ Tab D │  …        │
│  ╰───────╯ ╰───────╯ ╰───────╯ ╰───────╯           │
└────────────────────────────────────────────────────┘

┌─ level="module" ───────────────────────────────────┐
│  ╭─────╮ ╭─────╮ ╭─────╮ ╭─────╮                   │
│  │Tab A│ │Tab B│ │Tab C│ │Tab D│      [⚙ action]   │
│  ╰─────╯ ╰─────╯ ╰─────╯ ╰─────╯                   │
└────────────────────────────────────────────────────┘
```

| Part | Required | Description |
|------|----------|-------------|
| Tab Label | MUST | Text identifying the panel content, wrapped in a capsule shape |
| Active Capsule | MUST | Filled capsule marking the currently selected tab |
| Row Container | MUST | Horizontal strip holding all tab items, single-line |
| Overflow Entry | Conditional | Appears automatically when the row does not fit (see §6) |
| Right-side Action Icon | Optional | A single action (filter / sort / settings) placed at the far right of the row (see §5b) |

---

## 5. Variants Overview

### 5a. Level Variants

| `level` | Typical Placement | Inactive Style |
|---------|-------------------|----------------|
| `"page"` | Under a page title or under line-tabs, switches a sub-section of the page | Secondary capsule |
| `"module"` | Top of a card / widget, switches the module's own primary view | Outline capsule |

Designers MUST choose the `level` based on the tab's **position in the page hierarchy**, not on visual preference.

### 5b. Content Variants

pill-tabs at either level supports two content layouts:

| Sub-variant | Description | When |
|-------------|-------------|------|
| **Plain pill tabs** | Only the tab row, no trailing element | Default |
| **Pill tabs + right-side action icon** | A single icon-only action pinned to the right end of the row (e.g. filter, sort, view-options) | When the module needs one persistent action tied to the current tab |

Rules for the right-side action icon:

- MUST be a **single** icon-only button — do not stack multiple icons
- MUST represent an action that applies to the **currently active panel**, not tab-level navigation
- The action is **not** a tab and MUST NOT appear selected
- If the tab row overflows, the action icon MUST remain visible (the tab row collapses before the icon does)

---

## 6. Behavior

### Selection

- Exactly **one** tab must be selected at all times
- Clicking an unselected tab switches the content panel immediately
- Clicking the already-selected tab is a no-op (must not deselect)

### Overflow (when tabs do not fit)

The component handles overflow visuals automatically. Designers only need to confirm that **when tabs cannot all fit in the available width, the overflow behavior provided by the component is acceptable** for the scenario:

- On Web, the component collapses overflowed tabs behind an overflow entry provided by the component
- On Mobile, the tab row becomes horizontally scrollable / swipeable
- The exact visual (ellipsis vs "More" text, icon, etc.) is determined by `level` and platform and is **not a designer decision** — do not redesign it

If a layout cannot accept any overflow treatment (e.g. an extremely narrow fixed container), reduce the number of tabs instead of forcing a different component.

### Keyboard

- `ArrowLeft` / `ArrowRight`: move focus between tabs
- `Home` / `End`: move to first / last tab
- `Tab`: move focus out of the tablist

### SEO / Deep Linking

- pill-tabs supports rendering each trigger as an `<a>` tag via the `href` prop
- Use this when tabs should be directly linkable / indexable (e.g. public-facing content pages)

---

## 7. Configurable Options

The following decisions should be made explicitly by the designer; other visual details are handled by the component.

| Option | Purpose | Typical Values |
|--------|---------|----------------|
| `level` | Hierarchy level of the tab row | `"page"` (page secondary) / `"module"` (module primary) |
| `defaultValue` / `value` | Which tab is active on load / controlled value | tab `value` string |
| `href` (per trigger) | Render trigger as `<a>` for SEO / deep linking | URL string |
| Right-side action icon | Whether a single trailing icon-only action is attached to the row | present / absent |

Designers MUST decide explicitly:

- Which **level** (§5a) applies based on page hierarchy
- Whether a **right-side action icon** (§5b) is needed, and which single action it represents
- Whether triggers need `href` for SEO / deep linking

---

## 8. Do / Don't

| | Practice | Reason |
|---|---------|--------|
| DO | Use `level="page"` for page-level secondary switchers | Matches intended hierarchy + inactive style |
| DO | Use `level="module"` for the primary switcher at the top of a self-contained module | Outline style distinguishes module-level context |
| DO | Use pill-tabs for related slices of one content domain | Capsule shape signals "same domain, different slice" |
| DO | Let the component handle overflow automatically | Overflow visuals are level- and platform-specific by design |
| DO | Keep the right-side action icon to a **single** icon-only button | Prevents the row from competing with the tabs |
| DON'T | Use pill-tabs for independent page sections | Use line-tabs — pill-tabs implies sibling slices, not independent modules |
| DON'T | Stack two pill-tabs rows at the same hierarchy level | Creates ambiguous selection state |
| DON'T | Put multi-icon action clusters beside the tab row | Use a single trailing icon; move extra actions into the panel |
| DON'T | Redesign the overflow visual per page | The component's default overflow behavior is authoritative |
| DON'T | Use pill-tabs for actions or multi-select filters | Use Button / Checkbox group instead |
| DON'T | Allow zero selected tabs | pill-tabs is single-select — always one active |
