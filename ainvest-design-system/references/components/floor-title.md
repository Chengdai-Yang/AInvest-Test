# Guidelines — floor-title

> **Component Spec:** `ainvest-design-system/references/components/floor-title.md`

---

## 0. Document Role

This document covers **when and how to use** the floor-title (楼层标题) component. It focuses on decisions a designer must make explicitly — visual details handled automatically by the component are omitted.

- Component import: `@ainvest/floor-title`

---

## 1. Definition

FloorTitle is a **structural heading component** for building page hierarchy. It divides page content into titled sections (楼层), each with a title, optional description, and optional action bar.

The component maps directly to the page structure hierarchy system:

| FloorTitle Level | Structural Role | Semantic Name |
|-----------------|----------------|---------------|
| `level={1}` | Page title (页面标题) | **Pagetitle** |
| `level={2}` | Group title (分组标题) | **Grouptitle** |
| `level={3}` | Floor/section title (楼层标题) | **Floortitle** |

---

## 2. When to Use

| Condition | Use FloorTitle? |
|-----------|----------------|
| The page needs a top-level heading that identifies the entire page | YES — `level={1}` as Pagetitle |
| The page has content groups that each need a heading | YES — `level={2}` as Grouptitle |
| A content group contains independent sections/floors that each need a heading | YES — `level={3}` as Floortitle |
| A content module needs a navigable title with "see all" or link behavior | YES — with `titleIcon` (arrow) |
| A section title needs collapsible description or content | YES — with `collapseMode` |
| A section title has associated toolbar actions (chart, compare, filter, date) | YES — with `actionBarElement` |

### MUST

- MUST use FloorTitle (not raw heading tags) for all page-level, group-level, and floor-level headings in the AInvest product
- MUST select the level based on page structure type (see §4)
- MUST pair with the correct structural spacing (see §6)

### SHOULD

- SHOULD use `titleIcon` with a right-arrow (`>`) when the title links to a dedicated sub-page
- SHOULD use `description` when the section needs a short explanatory subtitle

---

## 3. When NOT to Use

| Condition | Use FloorTitle? | Alternative |
|-----------|----------------|-------------|
| Module-internal label inside a card (e.g. "Chart", "Stats") | NO | H4 text (18px SemiBold) — `typography.title.app.sm` |
| Tab label or filter label | NO | Tab / Filter component |
| Inline section heading within body text | NO | Semantic HTML heading or bold text |
| Navigation item | NO | Nav / Tab component |
| Page title in a modal or dialog | NO | Modal title pattern |

### MUST NOT

- MUST NOT use FloorTitle for decorative headings inside cards or modules — it is for page-structure-level headings only
- MUST NOT skip levels (e.g. use `level={1}` and `level={3}` without `level={2}` between them)
- MUST NOT use `level={1}` (40px) on L1 or L2 pages — only L3 pages use Pagetitle at 40px

---

## 4. Level Selection by Page Structure Type

FloorTitle level MUST be chosen based on the page structure type defined in `patterns/page-structure-types.md`.

### Title Type × Page Structure Matrix

| Title Type | Font Size | L1 Single-Focus | L2 Modular | L3 Multi-Group |
|------------|-----------|:-:|:-:|:-:|
| 一级标题 Pagetitle (`level={1}`) | 40px | | | YES |
| 二级标题 Grouptitle (`level={2}`) | 32px | YES (as page title) | YES (as page title) | YES (as group title) |
| 三级标题 Floortitle (`level={3}`) | 24px | | YES | YES |

### Critical Rules

> **L1 and L2 pages MUST use `level={2}` (Grouptitle, 32px) as their page title. MUST NOT use `level={1}` (Pagetitle, 40px).**
>
> Only L3 pages use `level={1}` (Pagetitle, 40px) as the page title.

### Decision Tree

```
What page structure type is this?
├── L1 Single-Focus
│   └── Page title: level={2} (32px)
│       └── (no floor titles; content is a single block)
│
├── L2 Modular Content
│   ├── Page title: level={2} (32px)
│   └── Floor titles: level={3} (24px)
│
└── L3 Multi-Group Complex
    ├── Page title: level={1} (40px)
    ├── Group titles: level={2} (32px)
    └── Floor titles: level={3} (24px)
```

---

## 5. Anatomy

### Component Structure

```
┌─────────────────────────────────────────────────────────┐
│  [Title Text] [TitleIcon]          [ActionBarElement]   │
│  [Description]                                          │
└─────────────────────────────────────────────────────────┘
```

| Part | Required | Description |
|------|----------|-------------|
| Title text | MUST | The heading text; size determined by `level` prop |
| Title icon (`titleIcon`) | Optional | Icon right of title text; typically a right-arrow `>` (link) or down-arrow `v` (collapse) |
| Description | Optional | Subtitle/explanation text below the title; toggleable via `collapseMode` |
| Action bar (`actionBarElement`) | Optional | Right-aligned toolbar; contains icon-only actions (chart, compare, share, filter, date picker) |

### Title Icon Behavior

The title icon indicates interactivity:

| Icon | Behavior | Usage |
|------|----------|-------|
| Right arrow `>` (link) | Title is clickable; navigates to sub-page | Navigable section headings; "see all" behavior |
| Down chevron `v` (collapse) | Title is clickable; toggles description or content visibility | Collapsible sections |
| No icon | Title is static text; not clickable | Static section headings |

### Action Bar Element Types (from Figma)

| Action Element | Icon | Usage |
|----------------|------|-------|
| Chart icon (`~`) | Line chart | Navigate to chart view / toggle chart display |
| Compare icon (`00`) | Side-by-side | Navigate to comparison view |
| Share icon | Share arrow | Share this section's content |
| Filter button | `≡ Filter` | Open filter panel for this section |
| Date picker | `Today v` | Select date range for this section |

### Rules

- Action bar elements MUST be icon-type elements — MUST NOT place full buttons or complex controls
- When page width > 500px: action bar is right-aligned on the same row as the title
- When page width <= 500px: action bar remains right-aligned; title font size decreases; action icons may reduce in size

---

## 6. Spacing Rules

FloorTitle does NOT provide inter-level spacing internally. The page layout MUST apply the following spacing values between structural landmarks.

> **Authoritative source:** `page-structure-types.md §Shared Spacing System` defines the canonical spacing values for structural landmarks. This section restates them for FloorTitle usage — if conflicts are ever detected, `page-structure-types.md` wins.

### Structural Spacing Table

| Spacing ID | From → To | Value | Token |
|------------|-----------|-------|-------|
| space1 | Nav bottom → page title (FloorTitle) | **48px** | `web-spacing-gap6` |
| E | Title text → description (when present) | 8px | `web-spacing-gap2` |
| space2 | Page title → first content / floor title (no description) | 24px (L1) / **32px (L2 override)** / 32px (L3) | `web-spacing-gap4` / gap5 |
| space2-alt | Description → first content / floor title (with description) | 32px (L1) / **48px (L2 override)** / 48px (L3) | `web-spacing-gap5` / gap6 |
| space3 | Floor/section title → its content | 24px | `web-spacing-gap4` |
| space4 | Floor → next floor (same-level siblings) | **48px** | `web-spacing-gap6` |
| space5 (L3 only) | Group → next group | 64px | — |

### Spacing by Page Structure Type

| Spacing | L1 Single-Focus | L2 Modular | L3 Multi-Group |
|---------|:-:|:-:|:-:|
| space1: nav → page title | 48px | 48px | 48px |
| E: title → description | 8px | 8px | 8px |
| space2: page title → content (no description) | 24px | **32px** | 32px |
| space2-alt: description → content (with description) | 32px | **48px** | 48px |
| space3: floor title → its content | 24px | 24px | 24px |
| space4: floor → floor | — (L1 has no floor-to-floor) | **48px** | 48px (within a group) |
| space5: group → group | — | — | 64px (L3 only) |

### space2 Decision Logic

```
Does the page title have a description directly beneath it?
├── YES → title → description = 8px (E)
│         description → content / next floor title = space2-alt (see table)
└── NO  → title → content / next floor title = space2 (see table)
```

---

## 7. Floor Composition

A complete floor module is composed of: **FloorTitle + Content + (optional) Action Link**.

### Floor Module Structure

```
┌─────────────────────────────────────────────────┐
│  FloorTitle >                    [~ 00]         │  ← Floor title with action bar
│  Description text xxxxxxxx.                     │
│                                                 │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐        │  ← Content area
│  │  card   │  │  card   │  │  card   │        │
│  └─────────┘  └─────────┘  └─────────┘        │
│                                                 │
│  View all >                                     │  ← Action link (optional)
└─────────────────────────────────────────────────┘
```

### Action Link Types

Below the content area, a floor may include an action link:

| Action Link | Icon | Behavior | Usage |
|-------------|------|----------|-------|
| "View all >" | Right arrow `>` | Navigate to full list page | Content list has more items than shown |
| "View more v" | Down chevron `v` | Expand to show more items inline | Content can expand within the current page |

### Rules

- Action links sit below the floor content, left-aligned
- "View all >" and "View more v" are mutually exclusive — use one per floor
- "View all >" navigates away; "View more v" expands in-place
- Action link typography: `typography.body.secondary.medium`, `color.text.primary`

---

## 8. Layout Composition by Page Type

### L1 — Single-Focus Page

```
nav
│  48px (space1)
FloorTitle level={2} "Grouptitle >"          [~ 00]
│  8px (E)
Description text
│  32px (space2-alt, L1) — content is single block
Core Content Block
```

- Uses `level={2}` (Grouptitle, 32px) as page title
- Only one FloorTitle instance; content below is a single unified block
- action bar with chart/compare icons if applicable

### L2 — Modular Content Page (Group + Floor)

```
nav
│  48px (space1)
FloorTitle level={2} "Grouptitle >"          [~ 00]
│  8px (E)
Description text
│  48px (space2-alt, L2 override)
FloorTitle level={3} "Floortitle >"          [Today v]
│  24px (space3)
┌─────────┐  ┌─────────┐  ┌─────────┐
│  card   │  │  card   │  │  card   │
└─────────┘  └─────────┘  └─────────┘
View all >
│  48px (space4)
FloorTitle level={3} "Floortitle"
│  24px (space3)
┌─────────┐  ┌─────────┐  ┌─────────┐
│  card   │  │  card   │  │  card   │
└─────────┘  └─────────┘  └─────────┘
View more v
```

- Page title: `level={2}` (32px)
- Floor titles: `level={3}` (24px)
- Group-to-floor spacing: 48px (space2-alt when description present)
- Floor-to-floor spacing: **48px (space4)** — not 64; 64 is reserved for L3 group-to-group (space5)

### L3 — Multi-Group Complex Page

```
nav
│  48px (space1)
FloorTitle level={1} "Pagetitle >"
│  8px (E)
Description text
│  48px (space2-alt)
FloorTitle level={2} "Grouptitle"            [~ 00]
│  24px (space3)
Content block
│  64px (space5, group → group)
FloorTitle level={2} "Grouptitle"            [~ 00]
│  24px (space3)
Content block
│  48px (space4, floor → floor within group)
FloorTitle level={3} "Floortitle"            [~ 00]
│  24px (space3)
Content block
```

- Page title: `level={1}` (40px) — ONLY in L3
- Group titles: `level={2}` (32px)
- Floor titles: `level={3}` (24px)
- Page-to-group: 48px; group-to-content: 24px; floor-to-floor (within a group): 48px; **group-to-group: 64px (space5, L3 only)**

---

## 9. Responsive Behavior

### Width > 500px vs Width <= 500px

| Property | Width > 500px | Width <= 500px |
|----------|--------------|----------------|
| Font size | Full size per level (40/32/24px) | Reduced (font size decreases) |
| Action bar | Right-aligned, full size icons | Right-aligned, compact icons |
| Description | Below title, full width | Below title, full width |
| Title icon | Standard size arrow | Standard size arrow |
| Layout | Title + action bar in single row | Title + action bar in single row (may wrap) |

### Slot Adaptation

**一级-页面标题 (Pagetitle, `level={1}`):**
- Arrow configurable as link arrow (`>`) or expand chevron (`v`)
- Description toggleable
- Width <= 500px: font size decreases

**二级-分组标题 (Grouptitle, `level={2}`):**
- Arrow configurable as link arrow (`>`) or expand chevron (`v`)
- Description toggleable
- Right-side action icons (chart + compare, or share)
- Width <= 500px: font size decreases

**三级-楼层标题 (Floortitle, `level={3}`):**
- Arrow configurable as link arrow (`>`) or expand chevron (`v`)
- Arrow at <= 500px can be replaced with tooltip info icon (`ⓘ`)
- Description toggleable
- Right-side actions: chart+compare icons, share icon, Filter button, Today dropdown
- Width <= 500px: font size decreases

---

## 10. Content Guidelines

### Title Text

- MUST be a short, descriptive noun phrase identifying the section (e.g. "Market Overview", "Trending Stocks", "AI Picks")
- MUST NOT use verb phrases (e.g. "View Market Overview")
- SHOULD be 1–4 words
- Title text with `titleIcon` arrow implies clickability — the target MUST exist

### Description Text

- MUST be a single sentence or short phrase providing context
- SHOULD be under 80 characters
- MUST NOT repeat the title text
- Example: "Don't miss a trick with global real-time updates."

### Action Bar

- MUST contain only icon-type elements
- MUST NOT place full-width buttons, text links, or complex controls in the action bar
- Common patterns: chart icon + compare icon, share icon alone, Filter pill, date picker dropdown

---

## 11. Do / Don't

| | Practice | Reason |
|---|---------|--------|
| DO | Select FloorTitle level based on page structure type (§4) | Maintains correct hierarchy |
| DO | Use `level={2}` (32px) as page title on L1 and L2 pages | Only L3 uses `level={1}` (40px) |
| DO | Apply spacing values from §6 between FloorTitle and content | Component does not provide inter-level spacing |
| DO | Use right-arrow `>` titleIcon when section links to a sub-page | Communicates navigability |
| DO | Use icon-only elements in `actionBarElement` | Keeps the title row clean |
| DO | Use "View all >" for navigating to full list, "View more v" for expanding in-place | Correct semantic distinction |
| DO | Maintain level nesting order: 1 → 2 → 3 | Avoid hierarchy confusion |
| DON'T | Use `level={1}` (40px) on L1 or L2 pages | Only L3 Multi-Group pages use 40px |
| DON'T | Skip levels (e.g. `level={1}` directly to `level={3}`) | Breaks hierarchy |
| DON'T | Use FloorTitle for module-internal labels | Use H4 (18px SemiBold) instead |
| DON'T | Place complex controls (buttons, inputs) in `actionBarElement` | Action bar is for icons only |
| DON'T | Omit spacing between FloorTitle and content | The component does not handle structural spacing |
| DON'T | Use different spacing values than §6 defines | Spacing is system-level, not arbitrary |

---

## 12. Decision Table

| Context | Level | Title Icon | Description | Action Bar | Action Link Below Content |
|---------|-------|------------|-------------|------------|--------------------------|
| L1 page heading | `level={2}` | `>` (link) or none | Optional | chart + compare icons | — |
| L2 page heading | `level={2}` | `>` (link) or `v` (collapse) | Optional | chart + compare icons, or share | — |
| L2 floor section | `level={3}` | `>` (link) or `v` (collapse) | Optional | chart+compare, share, Filter, Today | "View all >" or "View more v" |
| L3 page heading | `level={1}` | `>` (link) | Optional | — | — |
| L3 group heading | `level={2}` | `>` (link) or none | Optional | chart + compare icons | — |
| L3 floor section | `level={3}` | `>` (link) or none | Optional | chart+compare, share, Filter, Today | "View all >" or "View more v" |

---

## 13. Validation Checklist

- [ ] FloorTitle level matches page structure type (§4 matrix)
- [ ] L1 / L2 pages do NOT use `level={1}` (40px)
- [ ] L3 pages use `level={1}` for page title, `level={2}` for groups, `level={3}` for floors
- [ ] No levels are skipped (1 → 2 → 3)
- [ ] space1 (nav → page title) = **48px**
- [ ] space2 / space2-alt correctly applied based on description presence and page type (L2 override: space2=32, space2-alt=48)
- [ ] space3 (floor title → content) = 24px
- [ ] space4 (floor → floor, same-level siblings) = **48px** (NOT 64 — 64 is reserved for L3 group→group as space5)
- [ ] space5 (group → group) = 64px, L3 only
- [ ] Action bar contains only icon-type elements
- [ ] Title text is a noun phrase, not a verb phrase
- [ ] "View all >" and "View more v" used correctly per navigability intent
- [ ] Responsive: font size decreases at width <= 500px

---

## 14. Related Documents

- `ainvest-design-system/references/components/floor-title.md` — Component style spec (组件样式定义、层级、折叠模式、API、代码示例)
- `ainvest-design-system/references/patterns/page-structure-types.md` — Page structure classification (L1/L2/L3) and title hierarchy rules
- `ainvest-design-system/references/patterns/modular-content.md` — L2 page pattern and floor module composition
- `ainvest-design-system/SKILL.md` — Unified skill file
- `ainvest-design-system/references/typography.md` — Title token definitions (40px/32px/24px)
- `ainvest-design-system/references/spacing.md` — Web semantic spacing tokens
