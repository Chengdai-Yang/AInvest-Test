---
name: ainvest-design-system
description: >
  Ainvest financial product design system. Use when generating UI for Ainvest
  product pages. Provides design tokens (color, typography, spacing, radius,
  shadow), component specs, and layout rules for financial product interfaces.
component_source: references/components/
related:
  - references/rules/
  - references/tokens/color.md
  - references/tokens/typography.md
  - references/tokens/spacing.md
  - references/tokens/radius.md
  - references/tokens/shadow.md
---

# Ainvest Design System

Generate UI for Ainvest financial products by following these rules.
Do NOT generate from generic habits. Do NOT invent styles when this system already defines them.

---

## 0. Skill Scope — When to Use

Use this skill in the following cases:

- Building any AInvest-related website or web application
- Creating landing pages, dashboards, or marketing pages for AInvest
- Designing UI components that must follow AInvest brand conventions
- Converting designs or prototypes into code that matches AInvest visual standards
- Any time the user mentions "AInvest", "design system", or requests brand-aligned design
- Any web design work that must follow an existing brand specification

Out of scope: generic non-AInvest product UI, purely internal tooling with no brand constraints.

---

## Table of Contents

1. [Inputs](#1-inputs)
2. [Key Parameters](#2-key-parameters)
3. [Task Mode Detection](#3-task-mode-detection)
4. [Generation Workflow](#4-generation-workflow)
5. [Demo Restyling Workflow](#5-demo-restyling-workflow)
6. [Global Constraints](#6-global-constraints)
7. [Token Reference Index](#7-token-reference-index)
8. [Rule Reference Index](#8-rule-reference-index)
9. [Component Reference Index](#9-component-reference-index)
10. [Prohibited](#10-prohibited)
11. [Validation Checklist](#11-validation-checklist)
12. [Failure Rule](#12-failure-rule)
13. [Writing Rules for This System](#13-writing-rules-for-this-system)
14. [Preview & Delivery Rules](#14-preview--delivery-rules)

---

## 1. Inputs

To generate a page, you need exactly TWO inputs:

1. **Page Spec** — describes WHAT to build (structure, elements, interactions, states)
2. **This design system** — describes HOW to style

Priority when conflicts exist: **Page Spec > design system**.

The AI does NOT need the PRD. Page Spec + this design system is sufficient.

---

## 2. Key Parameters

| Parameter              | Value                                              |
|------------------------|----------------------------------------------------|
| Font                   | Plus Jakarta Sans                                  |
| Spacing base unit      | 8px (scale: 0, 2, 4, 8, 12, 16, 20, 24, 32, 40, 48) |
| Breakpoints            | XL >= 1410px / LG 900-1410px / MD 500-900px / SM < 500px |
| Fixed-width presets    | 1560px (default) / 1164px (medium) / 768px (reading) |
| Default button shape   | Full Rounded / pill (border-radius: 9999px)        |
| Button sizes           | XS 24px / S 28px / M 36px / L 44px                |
| Two-column gap         | 48px                                               |
| Two-column ratio       | 8:4 (default)                                      |
| Adaptive max width     | 2530px (limited adaptive only)                     |

---

## 3. Task Mode Detection

Before starting any work, identify which mode applies. The mode determines which workflow to follow.

### Mode A — Generate from Scratch

**Signals (any one is sufficient):**
- User provides a Page Spec document (or asks you to write one first)
- User says "生成", "帮我做一个", "build", "create", or "design" a new page
- No existing HTML file is referenced
- Task is described in terms of what the page should *contain*, not what it should *look like*

**→ Follow [§4 Generation Workflow](#4-generation-workflow)**

---

### Mode B — Restyle Existing Demo

**Signals (any one is sufficient):**
- User provides a path to an existing HTML file
- User says "改一下", "调整", "优化", "修一下", "restyle", "reskin", "apply design system to"
- User explicitly references "V1", "原版", "这个文件", or pastes an existing filename
- Task is described in terms of what currently *looks wrong* or *doesn't match* the design system

**→ Follow [§5 Demo Restyling Workflow](#5-demo-restyling-workflow)**

---

### When Unsure

If the user's intent is ambiguous, ask one clarifying question:
> "你是要基于现有文件修改，还是从头生成一个新页面？"

Do NOT proceed to generation without identifying the mode.

---

## 4. Generation Workflow

For every page, follow this exact order:

### Step 1 — Read Page Spec
Understand page purpose, structure, sections, components, and states.

### Step 2 — Determine Layout Framework
Load `references/rules/layout-grid.md`, then decide:
- Fixed-width (1560 / 1164 / 768) or Adaptive-width (limited / unlimited)?
- Single-column or multi-column?

### Step 3 — Apply Tokens (in order)
1. **Spacing** → establish layout rhythm (load `references/tokens/spacing.md`)
2. **Typography** → establish text hierarchy (load `references/tokens/typography.md`)
3. **Color** → apply semantic meaning (load `references/tokens/color.md` for token names and rules; **MUST read color values from `assets/tokens/color.json` only** — never copy hex from `.md`)
4. **Radius** → apply component shapes (load `references/tokens/radius.md`)
5. **Shadow** → apply elevation where needed (load `references/tokens/shadow.md`)
6. **Stroke** → apply border widths and colors (load `references/tokens/stroke.md`)

> **Color value source rule (CRITICAL):**
> `color.md` defines token names, categories, and usage rules — it does NOT contain hex values.
> All color values live in `assets/tokens/color.json`.
> Any code / CSS generation MUST read values from `color.json` and MUST NOT hard-code hex in generated files.

### Step 4 — Load Image & Asset Rules (MANDATORY before any component build)

**ALWAYS load `references/rules/image-asset-usage.md` before writing any component that contains an image.**

This is NOT optional. The trigger conditions are automatic:

| If the page contains...                              | You MUST read `image-asset-usage.md` and apply its rules |
|------------------------------------------------------|----------------------------------------------------------|
| A stock ticker (AAPL, NVDA, BTC, ETF, etc.)          | → Use CDN URL pattern + circular mask                    |
| A person avatar (politician, analyst, author, guru)  | → Use circular mask + `object-fit: cover`                |
| A news / article thumbnail                           | → Use `border-radius: var(--radius-sm)` (6px)            |
| Any `<img>` element                                  | → Set explicit width/height, `display: block`, fallback  |
| A national flag, sector icon, broker logo            | → Use CDN URL pattern from §6 Asset Registry             |

**CDN URL patterns** (from `image-asset-usage.md §6`):

```
Stock logo:    https://cdn.ainvest.com/icon/us/{SYMBOL}.png        ← .png ONLY, never .svg
ETF logo:      https://cdn.ainvest.com/icon/us/etf/{SYMBOL}.png    ← .png ONLY, never .svg
Crypto:        https://cdn.ainvest.com/icon/crypto/{SYMBOL}.png    ← .png ONLY, never .svg
National flag: https://cdn.ainvest.com/icon/national-flag/{CODE}.png
Broker logo:   https://cdn.ainvest.com/icon/brokers/{NAME}.png
```

> **SVG BANNED for financial icons**: CDN `.svg` files for stocks/crypto/ETFs render as solid color blobs, not real logos. Always use `.png`.

**Politician avatar local files** (for HTML pages in `temp/`):

```
Path pattern: assets/avatars/politicians/{Firstname_Lastname}.png
(requires symlink: temp/assets → ainvest-design-system/assets — run once: ln -s .../ainvest-design-system/assets .../temp/assets)

Available files:
  Nancy_Pelosi, Marjorie_Taylor_Greene, Ro_Khanna, Sheldon_Whitehouse,
  Shelley_Moore_Capito, Markwayne_Mullin, Tina_Smith,
  Debbie_Wasserman_Schultz, Cleo_Fields, Delaney_April_McClain,
  Gil_Cisneros, Jonathan_Jackson
```

For politicians NOT in the list above: render initials fallback — colored circle using party color (D = #165DFF, R = #FF381A) + 2-letter initials. Do NOT use CDN politician avatar URLs.

**Shape rules** (non-negotiable):
- Ticker logos → `border-radius: 50%` + `overflow: hidden` on the **container**, not the `<img>`
- Person avatars → same as ticker logos
- News thumbnails → `border-radius: var(--radius-sm)` (6px), NOT circular
- All images → `object-fit: cover`, `display: block`, explicit `width` + `height`
- All containers → `background: var(--color-bg-weak)` as fallback for load failures

**FAILURE conditions**:
- If any ticker logo renders as a solid colored circle → you used `.svg`; switch to `.png`
- If any ticker logo, avatar, or thumbnail is rendered as a square → `overflow: hidden` missing on container
- If a politician avatar is broken/empty → use initials fallback with party color

### Step 5 — Build Components
For each component in the Page Spec:
1. Check if a component doc exists in `references/components/`
2. If yes, follow it exactly
3. If no, use the closest existing pattern — do NOT invent new styles
4. Check `references/rules/component-selection.md` for Ainvest-specific selection rules

**Card components — MANDATORY extra step:**
When the page contains any card component (any contained surface with `border-radius` and `cursor: pointer`, or any layout unit described as a "card" in the Page Spec):
1. Load `references/rules/card.md` FIRST — it defines background, border, radius, padding, internal spacing, hover, active, and selected states all in one place
2. Do NOT go to `shadow.md` to decide card shadow — `card.md` defines the rule (no shadow; use border instead)
3. Do NOT invent card styles outside what `card.md` defines

### Step 6 — Cross-Page Consistency Review (MANDATORY before output)

**Before finalizing any page, check all shared UI elements against cross-page rules.**

A "shared UI element" is any module or component that appears (or is likely to appear) on ≥2 Ainvest pages. Examples:

| Shared Element | Check |
|---|---|
| AI Score / AI Rating badge | Same token, same shape, same size scale across all pages |
| Ticker identity block (logo + symbol + name) | Circular mask, same font size/weight, same spacing |
| Politician / person identity block | Same avatar size, same name font, same party badge |
| Return / gain % tag | Same color token (`color.price.up` / `color.price.down`), same format |
| Section title + subtitle | Same typography tokens, same spacing above/below |
| Filter bar / tab bar | Same height, same token, same active state pattern |
| Empty state | Same illustration placeholder pattern, same copy style |
| Pagination / load-more | Same token, same position |

**Consistency review process:**

1. **Identify shared elements** in the page being built — list every element that exists on ≥2 pages
2. **Check `spec-conventions/cross-page-rules.md`** — if a rule exists for this element, follow it exactly
3. **Check existing HTML pages in `temp/html/`** — visually verify the same element looks consistent
4. **If inconsistency found** → fix the current page to match the established pattern; if no pattern exists yet, define it in `spec-conventions/cross-page-rules.md`
5. **Report any cross-page rule additions** in the final response under Rule Change Reporting Convention

**MUST NOT** output a page where a shared element uses different visual treatment from already-published pages without a documented justification.

### Step 7 — Validate
Run through [§11 Validation Checklist](#11-validation-checklist) before finalizing.

---

## 5. Demo Restyling Workflow

Use this workflow when [§3 Task Mode Detection](#3-task-mode-detection) identifies **Mode B — Restyle Existing Demo**.

---

### Step A — Pre-Edit Audit (Do this FIRST, before touching any code)

Read the existing HTML file completely. Catalogue the following in a structured list:

| Category | What to Record |
|---|---|
| **Interactive functions** | All click handlers, toggles, tabs, accordions, modals, dropdowns, hover states |
| **Core layout structure** | Column counts, sidebar presence, content order (top to bottom), sticky elements |
| **Data fields** | Every displayed value — numbers, labels, strings, placeholder text |
| **Component hierarchy** | Which elements are grouped together as a semantic unit |

Take a screenshot (save to `temp/screenshots/`) to lock in the visual baseline before any edit.

This catalogue is the **source of truth** for Step F verification. Do not skip it.

---

### Step B — Classify Every Issue

For each problem identified (from user feedback or your own audit), classify it as one of:

#### Pure Style Change
Token swap with no structural consequence. ALL of the following must be true:
- No DOM restructure (no element added, removed, or moved)
- No change to layout flow (does not cause reflow in any element's column, row, or stacking order)
- No change to interaction behavior (click handlers, hover logic, tab switching remain identical)
- Only visual properties change: color, font, spacing, radius, shadow, border

Examples of Pure Style:
- Replace `#007AFF` with `var(--color-brand-primary)`
- Change `font-size: 14px` to `var(--font-size-sm)`
- Change `border-radius: 4px` to `var(--radius-sm)`
- Change `padding: 10px 14px` to `padding: var(--spacing-8) var(--spacing-16)`

#### Structural / Layout / Interaction Change
Any change that does NOT meet all four Pure Style conditions above. This includes:

- Moving, removing, or reordering DOM elements
- Changing column/row layout (e.g., from 2-col to 1-col)
- Regrouping elements (e.g., placing thumbnail and title into a shared wrapper)
- Adding or removing a container
- Changing how a user interaction works

---

### Step C — Apply Pure Style Changes Directly

For all issues classified as **Pure Style**: apply immediately using the token system. No user confirmation needed.

Token application order: Spacing → Typography → Color → Radius → Shadow → Stroke

---

### Step D — Handle Structural Issues (Confirm Before Changing)

For each issue classified as **Structural / Layout / Interaction**:

1. **State the problem** — describe what is wrong and why it violates design principles
2. **Give a professional recommendation** — explain what the correct structure should be and why
3. **Ask for explicit confirmation before applying**

Format:

> **[Structural issue]** The Up Next card currently renders the thumbnail as a full-width block with the title and metadata floating outside it. This breaks visual grouping — the image is not perceived as belonging to the same card as its text.
>
> **Recommendation:** Wrap thumbnail + title + metadata into a single flex container inside the card, so they read as one unified unit.
>
> **Confirm to apply?** _(yes / no / show me alternatives)_

Do NOT apply structural changes until the user confirms.

---

### Step E — Generate Alternatives (When User Is Unsure)

If the user responds with uncertainty about a structural recommendation (e.g., "我不确定", "show me alternatives", "哪个好看"), generate:

1. **Version A** — keep original layout, apply only Pure Style fixes
2. **Version B** — apply the recommended structural fix
3. (Optional) **Version C** — a third variant if there is a meaningfully different approach

Each version saves as a separate file (`-Va.html`, `-Vb.html`, etc.) so the user can compare side-by-side.

---

### Step F — Post-Generation Review

After applying all approved changes, verify against the **Pre-Edit Audit catalogue from Step A**:

| Verification dimension | Check |
|---|---|
| **Functions present** | Every interactive function catalogued in Step A still works |
| **Layout preserved** | Column count, content order, sidebar presence unchanged (unless structural change was approved) |
| **Data intact** | Every data value from Step A still appears correctly |
| **Interactions working** | Click handlers, tab switching, toggles, hover states all function as before |
| **Token compliance** | All visual properties now use semantic tokens — no raw hex values remain |

If any verification fails → fix before outputting the final file.

---

### Step G — Modification Report

Every restyling output MUST include a modification report in the final response.

**Format:**

| Area | Change Type | Before | After |
|---|---|---|---|
| Button shape | Pure Style | `border-radius: 4px` | `var(--radius-full)` |
| Card background | Pure Style | `#FFFFFF` | `var(--color-bg-surface)` |
| Title font size | Pure Style | `22px` | `var(--font-size-2xl)` (24px) |
| Up Next card structure | Structural (approved) | thumbnail + text floating separately | unified flex unit inside card |

Rules for the report:
- List every change, not just structural ones
- Use actual token names in the "After" column — no raw hex
- Mark change type clearly: `Pure Style` or `Structural (approved)`
- Do NOT summarize with bullet points — the table is mandatory

---

### Version File Rule

**NEVER overwrite the original file or any previous version.**

Naming:
- Original: `{name}.html` → DO NOT TOUCH
- First restyle: `{name}-V2.html`
- Second iteration: `{name}-V3.html`
- Subsequent: increment by 1

---

## 6. Global Constraints

### MUST
- Use semantic tokens from this system for all visual properties
- Use Full Rounded (pill) button as default shape
- Use Black Primary button for standard product actions
- Use spacing to express hierarchy and grouping (not borders)
- Support Light / Dark mode via token switching
- Keep dense financial interfaces clean and scannable

### MUST NOT
- Use raw hex/rgb values — always use semantic tokens
- Invent new tokens (font sizes, weights, spacing, radius, shadow, colors)
- Use Square button as default (special cases only)
- Use Brand Blue button outside trading / paid-product / landing page contexts
- Use Landing Page styles in standard product pages
- Mix price/status/chart colors with UI action colors

### Design Philosophy
- Clarity over decoration
- Scan efficiency over novelty
- Structure over card stacking
- Professional trustworthiness over emotional hype
- Dense interfaces that remain clean and readable

### Perceptual UX Heuristics Enforcement (MANDATORY)

**After completing token application (§4 Step 3) and BEFORE building components (§4 Step 5):**

Load `references/rules/perceptual-ux-heuristics.md` and run the §1–§6 self-audit questions against the planned layout. This step is not optional and must not be deferred to the final review.

**Specific enforcement triggers** (any of these requires an immediate heuristics check):

| Trigger | Heuristic concern |
|---|---|
| Page has ≥3 visible sections | Visual hierarchy — does the eye know where to start? |
| Any card contains ≥3 data types | Grouping — are related items visually unified? |
| Any row mixes two different semantic categories | Proximity — are unlike things clearly separated? |
| Page has both price data and category tags | Differentiation — do dissimilar items look different? |
| Any component appears in ≥2 sizes on the same page | Consistency — does size variation carry intentional meaning? |
| User feedback says "looks crowded" or "hard to read" | Contrast / density — re-read §3 and §4 of the heuristics file |

If a heuristics check reveals a violation → fix the layout plan before writing component HTML.

---

## 7. Token Reference Index

Load these files from `references/tokens/` as needed during generation.
Only load what the current page requires.

| File             | Contains                                  | When to Load                      |
|------------------|-------------------------------------------|-----------------------------------|
| `color.md`       | Color token **names**, categories, and usage rules (no hex values) | Always (every page uses color)    |
| `typography.md`  | Font scale, weights, line heights, styles | Always (every page uses text)     |
| `spacing.md`     | 8px base unit scale, usage rules          | Always (every page uses spacing)  |
| `radius.md`      | Corner radius tokens per component type   | When generating shaped components |
| `shadow.md`      | Elevation tokens (web + app)              | When generating overlays, dropdowns, modals — NOT cards (see `card.md`) |
| `stroke.md`      | Border widths + colors, Web/App split, component isolation rules | When generating any element with a border or divider |

### Token Value Source

| File                        | Contains                                                            |
|-----------------------------|---------------------------------------------------------------------|
| `assets/tokens/color.json`  | **All color hex / rgba values** (light + dark). The only source of truth for color values. `color.md` is rules-only. |

---

## 8. Rule Reference Index

Load these files from `references/rules/` as needed.

| File                          | Contains                                                  | When to Load                                      |
|-------------------------------|-----------------------------------------------------------|---------------------------------------------------|
| `layout-grid.md`              | Breakpoints, grid, fixed/adaptive width, responsive rules | Always (every page needs layout)                  |
| `component-selection.md`      | Ainvest-specific component choice rules                   | When deciding which component to use              |
| `card.md`                     | Card visual style (bg, border, radius, padding) + interaction states (hover, active, selected) + carousel card counts by breakpoint | When page contains any card or carousel           |
| `color-opacity-on-overlap.md` | Solid fill rule for floating elements (carousel arrows, dropdowns, tooltips) positioned over borders or content | When any element overlaps visible content         |
| `list-table-pattern.md`       | List/table container rules, row hover, header styling, column alignment (numeric = right, text = left) | When page contains any data table or list         |
| `image-asset-usage.md`        | Shape rules for ticker logos (circular), avatars (circular), thumbnails (rounded rect); `object-fit` and fallback rules | When page contains any image — logos, avatars, thumbnails |
| `perceptual-ux-heuristics.md` | Perceptual UX principles: visual hierarchy, proximity/grouping, contrast, consistency, affordance — used during Pre-Design Protocol and consistency review | Always (read once at start of any page design; re-read when design feels off) |

### Skill Reference Index

Load these files from `skills/` as needed.

| File                    | Contains                                              | When to Load                                                  |
|-------------------------|-------------------------------------------------------|---------------------------------------------------------------|
| `page-spec-writer.md`   | Workflow + 7-section structure for writing Page Specs; P1–P4 page-level priority rules; L1–L3 module-level hierarchy rules; business-specific module layout format; UI Consistency Rules format; cross-page promotion rules | Before writing any Page Spec document for an Ainvest page     |

> This is the **authoritative source** for the page-spec-writer skill. The global OpenCode skill at `~/.config/opencode/skill/page-spec-writer/` is a stub that points here.

### Spec Convention Files

Accumulated knowledge from building Ainvest pages. Load as referenced by `page-spec-writer.md`.

| File                                           | Contains                                                                 | When to Load                                              |
|------------------------------------------------|--------------------------------------------------------------------------|-----------------------------------------------------------|
| `spec-conventions/information-priority.md`     | L1/L2/L3 type/size/weight token mapping; contrast signal rules; applied examples per card type | When writing Module-level Information Hierarchy blocks in any Section Spec |
| `spec-conventions/cross-page-rules.md`         | Consistency rules promoted from individual Specs; applies across ≥2 pages | When checking whether a UI Consistency Rule already exists cross-page |
| `spec-conventions/biz-module-patterns.md`      | Named layout patterns for business-specific modules; `PoliticianIdentityBlock` variants | When a Spec references a business-specific module by pattern name |

### Spec Template Files

Starting-point structures for common Ainvest page types. Load before writing a new Spec.

| File                                   | Page type                                                  | Typical examples                                   |
|----------------------------------------|------------------------------------------------------------|----------------------------------------------------|
| `spec-templates/carousel-feed-page.md` | Feed of carousels/grids; no sidebar; scroll-to-discover    | Congress Trading, News Hub, Discover Feed          |
| `spec-templates/list-page.md`          | Data table or ranked list; optional sidebar filter         | AI Stock Picks, Screener, Watchlist, Search Results |
| `spec-templates/tool-page.md`          | Interactive input tool + curated discovery below           | Compare ETFs, Portfolio Analyzer, AI Signal Explorer |
| `spec-templates/detail-page.md`        | Single-subject depth page; drill-in from list or search    | Stock AI Detail, Politician Profile, ETF Detail    |

---

## 9. Component Reference Index

Load these files from `references/components/` when the page contains the component.

| File                    | Contains                                                        | When to Load                                          |
|-------------------------|-----------------------------------------------------------------|-------------------------------------------------------|
| `breadcrumb.md`         | Breadcrumb hierarchy rules, collapse behavior, node states, platform breakpoints | When page contains any breadcrumb navigation          |
| `button.md`             | Button types (Full Rounded, Rect, Circle), hierarchy, sizes, colors, states | When page contains any standard product button        |
| `carousel.md`           | Carousel indicator types (dot/free-scroll/arrow), sizes, animation, overflow | When page contains any carousel or horizontal card slider |
| `checkbox-group.md`     | Checkbox variants, positioning, spacing, states                 | When page contains any multi-select control           |
| `collapse.md`           | Collapse variants (full-hide, half-hide, text, list), trigger rules, panel mode | When page contains any expandable/collapsible section |
| `date-picker.md`        | Date Picker props, view modes, navigation, action bar           | When page contains a single-date selector             |
| `date-range-picker.md`  | Date Range Picker props, range selection rules, action bar      | When page contains a date range selector              |
| `dialog.md`             | Dialog variants, sizes, button layout, platform differences     | When page contains any modal dialog                   |
| `drawer.md`             | Bottom Sheet types, header styles, height rules, responsive switching | When page contains a bottom sheet or drawer      |
| `dropdown-menu.md`      | Dropdown Menu trigger rules, single/multi-select, grouping, responsive switching | When page contains any dropdown or contextual menu    |
| `empty.md`              | Empty State levels, icon types, content configs, container setup | When page contains any empty or error state           |
| `floor-title.md`        | FloorTitle levels (page/group/section), action bar, collapse mode | When page contains any section heading or page title  |
| `input.md`              | Input variants (text, number, range, textarea, OTP), props, validation | When page contains any input or form field       |
| `landing-button.md`     | Landing Page Button sizes, colors, states                       | When page contains a landing page CTA button          |
| `line-tabs.md`          | Line tabs spacing variants, overflow patterns, sticky, responsive | When page contains any primary page-level tab navigation |
| `link.md`               | Link types (default/emphasis), icon usage, underline rules, color states | When page contains any inline text link               |
| `noticebar.md`          | Notice Bar hierarchy, background emphasis, right-slot config, responsive rules | When page contains any notice bar or announcement banner |
| `notification.md`       | Notification variants, stacking rules, position, auto-dismiss behavior | When page contains any floating notification card     |
| `pagination.md`         | Pagination variants, page-number display logic, platform constraints   | When page contains any paginated list or table        |
| `pill-tabs.md`          | Pill tabs level prop, content variants, right-side action icon, overflow | When page contains any secondary or module-level tab switcher |
| `popover.md`            | Popover variants, step indicators, positioning, trigger behavior       | When page contains any guided tooltip or onboarding bubble |
| `radio-group.md`        | Radio variants, positioning, spacing, states                    | When page contains any single-select control          |
| `search-input.md`       | Search input platform variants, states, dropdown rules, Cancel behavior | When page contains any search or query input field    |
| `segment-tabs.md`       | Segment tabs label layout, dropdown caret, overflow strategies  | When page contains any chart timeframe or dense module switcher |
| `spinner.md`            | Spinner variants, sizes, loading scenarios (page/pull/load-more/inline) | When page contains any loading indicator              |
| `switch.md`             | Switch states, sizes, label rules, loading and confirmation behavior   | When page contains any binary toggle control          |
| `table.md`              | Table features (sort, pin, sticky, virtualization), Event Calendar variant, responsive | When page contains any data table or event calendar   |
| `tag.md`                | Tag categories, sizes, colors, stroke, interaction states       | When page contains any tag or label element           |
| `text-button.md`        | Text Button usage, colors, states                               | When page contains a text-only button on a landing page |
| `toast.md`              | Toast status types, position rules, auto-dismiss, stacking behavior    | When page contains any operation feedback message     |
| `tooltip.md`            | Tooltip variants, trigger modes, icon type rules, text layout          | When page contains any hover/click text explanation bubble |


---

## 10. Prohibited

- MUST NOT use raw hex values
- MUST NOT invent new semantic tokens
- MUST NOT invent new button styles, typography styles, spacing/radius/shadow values
- MUST NOT use Square button as default or on Mobile
- MUST NOT use Brand Blue button in non-trading, non-paid-product product UI
- MUST NOT use Text Button or Landing Page Button in product pages
- MUST NOT place multiple equally strong primary solid buttons in one section
- MUST NOT overuse Brand Blue
- MUST NOT ignore this design system when it defines a rule

---

## 11. Validation Checklist

Before finalizing output, verify each category:

### Breadcrumb (`references/components/breadcrumb.md`)
- Breadcrumb used only on pages at level 2 or deeper — not on top-level landing pages, modals, or flat SPA tools?
- Current (last) node is plain non-interactive text — not a clickable link?
- Root node and current node always visible — neither collapsed into `...`?
- At viewport ≤ 500px, collapsed aggressively to root + `...` + current?

### Button (`references/components/button.md`)
- All standard buttons follow `references/components/button.md`?
- Full Rounded (胶囊按钮) used as default shape?
- Rect (方形按钮) used only when container space is limited AND multiple short-text options appear together, or action is icon-only?
- Rect (方形按钮) absent from Mobile?
- Circle (圆形按钮) used only for icon-only entry points with no text label?
- Circle (圆形按钮) width and height equal (perfect circle)?
- Button radius uses `radius.pill` for Full Rounded, `radius.full` for Circle — NOT overridden by global `radius.md`?
- Rect radius matches size-specific values (Large: 10px / Medium+Small: 6px) — NOT overridden by global `radius.md`?
- No shadow applied to buttons in any state?
- Disabled state uses `opacity: 0.4` — no custom gray substituted?
- Hover uses semi-transparent black overlay — no `transform`, translate, or scale?
- All button colors use button-specific semantic tokens from `color.md` — no raw hex?

### Cards & Carousels
- Card default style follows `references/rules/card.md`? (bg: `color.bg.surface`, border: `1px solid color.divider.level3`, radius: `radius.md`, padding: `spacing.16`, no shadow)
- Card hover uses flat fill only (`#F5F5F5`)? No `translateY`, no box-shadow?
- Compact card carousel shows 3–6 cards? (XL=5, LG=4, MD=3)
- Carousel arrow hover uses solid fill (`#E5E5E5`)? Not `rgba`?

### Carousel (`references/components/carousel.md`)
- Dot Indicator used for fixed-page-count / snap carousels; Free-Scroll Indicator used for free-scroll content — not mixed?
- Left arrow hidden on first page, right arrow hidden on last page — no fade animation, instant toggle?
- Dot state switches instantly on page change — no dot transition animation?
- Arrow hover uses direct color/shadow change — no opacity transition or translateX?

### Checkbox Group (`references/components/checkbox-group.md`)
- Correct variant chosen: Basic (list), Binary (single boolean), Filter pills (filter bar), Special (settings panel)?
- Checkbox position on App: right when no right-side content, left when right-side content present?
- Checkbox position on Web: always left regardless of right-side content?
- Hover state absent from App — Web only?

### Collapse (`references/components/collapse.md`)
- Correct collapse variant chosen: full-hide (low priority module), half-hide (long content blocking view), text-fold (long paragraph), list (data list with `visibleCount`)?
- `Collapse.List` used for repeated data lists — not full-hide collapse?
- Half-hide gradient fade applied at the bottom edge — not an abrupt cut?
- Only one trigger per collapse — not multiple expand/collapse entry points for the same section?

### Color
- All colors from `color.md` semantic tokens?
- Semantic groups used correctly (not mixing price colors with UI action colors)?
- Light/Dark mode switching via tokens?

### Dropdown Menu (`references/components/dropdown-menu.md`)
- Dropdown Menu used for discrete action/selection lists — not for primary navigation or form submission?
- On web viewport > 500px, list-type Bottom Sheet switches to Dropdown Menu — not kept as a sheet?
- Menu panel triggered only by explicit user action — never appears automatically?
- Multi-select uses checkboxes inside the menu — not multiple separate trigger buttons?

### Date Picker (`references/components/date-picker.md`)
- Date Picker used for single-point selection only — date ranges use `date-range-picker` instead?
- Trading week context uses `viewMode="week"` with Mon–Fri trading week, not natural week?
- `locale` and `timezone` always set together — never one without the other?
- Action bar omitted when single-click auto-confirm is sufficient (not force-added by default)?

### Date Range Picker (`references/components/date-range-picker.md`)
- Action bar always includes Confirm + Cancel — auto-confirm never used for range selection?
- `dateRange` prop uses explicit `null` for unset start/end — not `undefined`?
- `locale` and `timezone` always set together — never one without the other?

### Dialog (`references/components/dialog.md`)
- Dialog used only when the user must act immediately — not for passive feedback or status messages?
- `variant="app"` used for mobile (no close button, full-width buttons); `variant="web"` for desktop?
- `size` chosen correctly: `sm` for simple confirmations, `md` for standard, `lg` for rich/dual-column content?
- `fullscreen="sm"` used only for functional/operation dialogs — not for simple confirmations or marketing?
- Primary button placed last in horizontal layout, first in vertical layout?
- Marketing Dialog image position: top on App, left side on Web?

### Drawer / Bottom Sheet (`references/components/drawer.md`)
- Bottom Sheet used for mobile and web ≤ 500px only — not forced on wide web viewports?
- List-type content switches to Dropdown Menu on web > 500px; marketing-type switches to Dialog?
- Header style matches interaction model: Back+Title+Close (drill-down), Cancel+Title+Button (filter/confirm), Title+Close (dismiss only)?
- Footer button always fixed at bottom — not scrolling with content?

### Empty State (`references/components/empty.md`)
- `EmptyLevel.Page` used for full-page states; `EmptyLevel.Module` used for section/card/widget states?
- `EmptyType.None` (no icon) used by default for `EmptyLevel.Module`?
- Parent element has `@container` / `container-type: inline-size` set — not skipped?
- `showAction={false}` set when no meaningful recovery action exists — no no-op button rendered?

### Floor Title (`references/components/floor-title.md`)
- Correct level used: `level={1}` for page title, `level={2}` for group title, `level={3}` for section/floor title?
- `titleIcon` (arrow) added only when the title is navigable (links to a list page or "see all")?
- `actionBarElement` used for toolbar actions (chart, compare, filter, date) — not placed as separate floating elements?
- At most one `level={1}` per page?

### Images & Assets
- All ticker logos circular? (`border-radius: 50%` + `overflow: hidden` on **container**)?
- All person avatars circular?
- All news thumbnails using `border-radius: var(--radius-sm)` (6px), NOT circular?
- All `<img>` elements have explicit `width` + `height` attributes?
- All `<img>` elements have `display: block` and `object-fit: cover`?
- All image containers have `background: var(--color-bg-weak)` fallback?
    - All stock ticker URLs using CDN pattern `https://cdn.ainvest.com/icon/us/{SYMBOL}.png`? (`.svg` is BANNED — renders as solid color blob)
- All image symbols match exact casing of the ticker (AAPL not aapl)?

### Input (`references/components/input.md`)
- Correct variant chosen for data type: `Input.Number` for numerics, `Input.RangeNumber` for ranges, `Input.Textarea` for multi-line, `Input.SingleOtp` / `Input.MultipleOtp` for verification codes?
- `Input.Number` uses `adjustType="button"` on web and `adjustType="arrow"` on mobile?
- Real-time validation implemented via `onChange` + `error`/`success` props — not expected as a built-in?
- `onSendCode` returns `false` on API failure to allow retry — not left stuck in "Sending..." state?
- Label always provided — placeholder never used as a substitute for a label?

### Landing Button (`references/components/landing-button.md`)
- Landing Page Button used only on Web landing pages — absent from standard product pages and App UI?
- Shape is always Full Rounded (`radius.pill`) — no square or custom radius?
- Brand Blue permitted for landing page primary CTA?
- No shadow applied in any state?
- Disabled state uses `opacity: 0.4` — no custom gray substituted?
- Hover uses semi-transparent black overlay — no `transform`, translate, or scale?
- All button colors use button-specific semantic tokens from `color.md` — no raw hex?

### Layout
- Layout follows `references/rules/layout-grid.md`?
- Correct fixed-width preset or adaptive type chosen?
- Alignment and grouping consistent?

### Line Tabs (`references/components/line-tabs.md`)
- Line tabs used for page-level primary navigation between independent sections — not for module-level switching (use pill-tabs)?
- Fixed-gap spacing used on Web — not equal-width at page level?
- Pattern A (arrows) used for small tab count overflow on Web; Pattern B (`...` menu) used for large tab count?
- On Web Pattern B, `...` dropdown shows only overflowed tabs — visible tabs not repeated inside?
- Sticky enabled when content below is long enough to scroll past the tabs?

### Link (`references/components/link.md`)
- Link used for navigation — not for actions that change state (use Button instead)?
- Emphasis variant used when the link needs stronger visual weight; Default for inline body text?
- Underline shown for legal/policy/disclaimer links — not omitted?
- External links open in a new tab; internal links use same-window navigation?

### Notice Bar (`references/components/noticebar.md`)
- Correct hierarchy chosen: 一级 (52px, full-width, page-top) for global messages; 二级 (42px, rounded, module-top) for module-level messages?
- Background emphasis matches message importance: Blue (warning/critical), Weak Blue (default/regular), Grey (quiet/persistent), Gradient (upgrade/promotional)?
- Close icon moved to the left when the right slot already has an action button or chevron?
- No more than one 一级 Notice Bar on the page at any time?
- Marquee used only in markets/trading contexts — not elsewhere?
- Web: text truncates to 1 line; Mobile/narrow: wraps up to `maxLine` (max 3), side margin 16px?

### Notification (`references/components/notification.md`)
- Notification used only for rich content (title + description, buttons, or image) — simple single-line feedback uses Toast instead?
- Close button (✕) always present — never omitted?
- Description kept to 2 lines maximum?
- Maximum 3 notifications visible simultaneously — stacking used beyond that?
- Default position: top-right on Web, top-center on Mobile?
- Swipe-to-dismiss disabled on Mobile — Web only?

### Pagination (`references/components/pagination.md`)
- Pagination used on Web only — replaced by `View more >` button on Mobile and viewport ≤ 500px?
- Hidden entirely when total pages = 1 (`hideOnSinglePage`)?
- Full Pagination (Variant B) used when page-size control is needed; Simple Pagination (Variant A) for list-only navigation?
- Placed at the bottom of its data area — not separated from it by other modules?
- `<` disabled on page 1; `>` disabled on last page; `...` non-interactive?
- Page size change resets current page to 1?

### Pill Tabs (`references/components/pill-tabs.md`)
- `level="page"` used for secondary switchers under a page title or line-tabs; `level="module"` for the primary switcher inside a card/widget?
- Pill tabs used for related slices of one content domain — not for independent page sections (use line-tabs)?
- Right-side action icon limited to a single icon-only button — not a cluster of multiple icons?
- Two pill-tabs rows not stacked at the same hierarchy level in one module?

### Popover (`references/components/popover.md`)
- Popover used only for rich content (title + body, image, or buttons) — simple single-line tips use Tooltip instead?
- Only one Popover visible at a time?
- Correct variant chosen: Single (1 step), Multiple Indicator (≤ 5 steps, dots), Multiple Counter (numbered steps), Custom (image/rich content), Custom White (dark UI context)?
- Description kept to 2 lines maximum?
- Arrow anchored to the trigger element; bubble position auto-adjusted to stay within viewport?
- Multi-step flow includes a Skip option?

### Radio Group (`references/components/radio-group.md`)
- Basic Radio used only when a confirm step follows — Special Selection List used for instant-apply?
- Standard Radio Filter used for filter bar pills — not Basic Radio?
- Radio indicator position follows same platform rules as Checkbox Group (App: context-dependent; Web: always left)?
- Hover state absent from App — Web only?

### Radius
- All corner radius values from `radius.md`?

### SearchBar (`references/components/search-input.md`)
- Search input used for search/query only — general form fields use Input instead?
- Web: 40px height, dropdown with 4px gap, no Cancel label; App: 36px height, no dropdown (navigate to results page), Cancel label on focus?
- Clear icon shown only when input has content — hidden when empty?
- Focus border uses `color.text.primary` — not brand blue?
- Dropdown hover uses stacked `color.interaction.hover` overlay — not solid fill?

### Segment Tabs (`references/components/segment-tabs.md`)
- Segment tabs used only at module level (inside a card/widget) — not as page-level navigation?
- All option labels short and uniform — no mixing of long and short labels?
- Single consistent option layout across all options (all label-only or all label+sub-value — not mixed)?
- Dropdown caret present on at most one option?

### Shadow
- All shadows from `shadow.md`?
- Elevation used only where semantically needed?

### Spacing
- All padding, gap, margin values from `spacing.md`?
- Spacing expresses hierarchy and grouping?

### Spinner (`references/components/spinner.md`)
- Correct scenario matched: page loading (centered + skeleton), pull-to-refresh, load-more (bottom), inline button (sm embedded)?
- At most one spinner per content zone?
- Spinner shown as soon as async starts and removed as soon as data arrives — not lingering after load?
- Skeleton used for placeholder content — not spinner?

### Switch (`references/components/switch.md`)
- Switch used only for immediate binary toggles — not for form selections (use Checkbox) or actions (use Button)?
- Label is a noun phrase describing what is controlled — no action verbs or consequence language?
- 32px size used by default — 24px/16px only in genuinely space-constrained contexts?
- Loading state shown during async operations; Switch non-interactive during loading?
- Dangerous or irreversible toggles require a confirmation dialog before executing — not immediate?

### Table (`references/components/table.md`)
- Table used for structured multi-column data with scan/sort/compare needs — not for card lists or single-record detail views?
- Event Calendar variant used when data is date-grouped — infinite scroll only, no pagination?
- `enableVirtualization` enabled for lists ≥ 100 rows?
- First column pinned only when horizontal scroll is required — not by default?
- `sortCycle="default"` (3-state) for columns with no default sort; `sortCycle="loop"` (2-state) for columns with a default sort?
- Viewport ≤ 500px switches to mobile stock-list style — not rendered in tabular form?

### Tag (`references/components/tag.md`)
> Tag has no developed component — all visual attributes must be manually implemented from the spec.
- Correct tag category chosen: 通用标签 (marking/classification), 业务标签 (functional/stock/recommendation), 文字标签 (inline text), or 自定义标签 (special highlight)?
- Correct size, padding, font, radius, icon size, stroke width, and color style per spec — no approximation or invention?
- Correct semantic color applied — blue not used for price movement; red not used outside live/price-down/warning/failure; gray switches between `#F2F2F2` (Light) and `#292929` (Dark)?
- Tag interactability matches the design intent — tag is non-interactive (informational only) or interactive (triggers navigation/filter/action)?
- If interactive: Hover = `#000` 5% overlay (Web only), Click = `#000` 10% overlay, arrow (>) present; if non-interactive: Default state only, no Hover or Click?

### Text Button (`references/components/text-button.md`)
- Text Button used only on Web landing pages — absent from standard product pages and App UI?
- No background, no border, no fill applied — text only?
- Hover uses token-driven feedback (underline or opacity shift) only — no background, border, `transform`, translate, or scale added?
- No shadow applied in any state?
- Disabled state uses `opacity: 0.4` — no custom gray substituted?
- All button colors use button-specific semantic tokens from `color.md` — no raw hex?

### Toast (`references/components/toast.md`)
- Toast used for simple single-line operation feedback only — rich content with title/description/buttons uses Notification instead?
- Status type icon matches feedback semantics (success/error/warning/info/loading)?
- Default position: top center on Web, bottom center on Mobile?
- Only one Toast visible at a time — no stacking?
- `duration: 0` used only when persistent display is intentional — not as a default?

### Tooltip (`references/components/tooltip.md`)
- Tooltip used for 1–2 line supplementary text only — rich content with title/image/buttons uses Popover instead?
- Correct variant: Primary (blue) for proactive new-feature tips; Neutral (black) for passive hover explanations?
- Mobile trigger is click — not hover (no hover on touch devices)?
- Only one Tooltip visible at a time; proactive tips appear only once per user?
- Info icon (①) used for static definitions; Help icon (②) used for interactive help links — not swapped?

### Typography
- All text uses styles from `typography.md`?
- Font size, weight, line height all from defined tokens?
- Text hierarchy is correct and consistent?

### Cross-Page Consistency
- All shared UI elements (AI Score, ticker block, person identity, return tags, section titles, filter bars) match the established cross-page pattern?
- `spec-conventions/cross-page-rules.md` consulted and up-to-date?
- No shared element uses a different visual treatment from already-published pages (unless change is documented)?

### Information Architecture
- Is the breadcrumb (if present) left-aligned with the page content container — not centered?
- Are semantically related elements grouped into a single visual unit? (Example: thumbnail + title + metadata must be one block, not three separate floating elements)
- Are elements of different data types visually distinguished? (Example: quantitative metrics like "1.2M views" vs. category tags like "Energy" must NOT share identical styling)
- Does each card or section communicate exactly one concept — not a mix of unrelated data types?
- Is there at most 1–2 primary visual focal points per screen? (If everything is emphasized, nothing is)
- Do related elements share a common alignment axis? (Left edge, or top edge — not arbitrary floating)
- Is the visual grouping of elements consistent with their semantic grouping in the data model?

If any check fails → fix that part before output.

---

## 12. Failure Rule

If rules from this system are violated:
1. Stop applying guessed or generic patterns
2. Re-read the relevant reference file
3. Regenerate using only this system's rules

---

## 13. Writing Rules for This System

When adding or editing files in this design system:

- **Only write rules the model does NOT already know.** Generic knowledge (e.g., "use Button for actions", "use Input for text entry") must be omitted. Every rule must be Ainvest-specific or counter-intuitive.
- Use MUST / MUST NOT / FAILURE structure in rule files
- Do NOT use raw values — define and reference semantic tokens
- Do NOT duplicate rules across files — each rule lives in one place
- Place new token definitions in the correct token file
- Place new component docs in `references/components/`
- Place new rules in `references/rules/`

---

## 14. Preview & Delivery Rules

### When to use HTTP server vs. open file directly

| Scenario | Method | Reason |
|---|---|---|
| Mock demo — all data hardcoded in HTML | **Open file directly** (`file://`) | No server needed; browser allows local resource access |
| Local images / fonts referenced by relative path | **Open file directly** | Relative paths work fine under `file://` |
| Page makes `fetch()` / XHR calls to any URL | **HTTP server required** | `file://` triggers CORS block on all fetch requests |
| ES Modules (`<script type="module">`) | **HTTP server required** | Chrome blocks ES module imports under `file://` |
| Service Worker registration | **HTTP server required** | SW only registers under `http://` or `https://` |
| Sharing preview link with others | **HTTP server required** | `file://` is local-only |

**Default rule for this project**: Generated pages in `temp/` are mock demos with hardcoded data. **Open directly in browser — do NOT start a server unless one of the above conditions applies.**

### MUST NOT
- Start an HTTP server for a pure static mock page
- Use `python3 -m http.server` or any dev server unless fetch/module/SW is involved
