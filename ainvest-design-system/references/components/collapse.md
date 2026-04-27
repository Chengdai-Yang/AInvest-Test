# Guidelines — Collapse (折叠面板)

> **Component Spec:** `ainvest-design-system/references/components/collapse.md`

---

## 0. Document Role

This document covers **when and how to use** the Collapse (折叠面板) component. It focuses on decisions a designer must make explicitly — visual details handled automatically by the component are omitted.

- Component import: `@ainvest/collapse`

---

## 1. Definition

Collapse (折叠面板) is a container that hides secondary, over-long, or auxiliary information to help the page show global classification or the content that follows. It balances **information density** with **page tidiness**: users can choose to unfold only what they need.

Compared with other information-density components:

| Component | Purpose |
|-----------|---------|
| Collapse | Hides / reveals bulk of secondary or overflow content inside an inline container |
| Tab | Switches between peer content groups, only one active at a time |
| Popover / Tooltip | Temporarily reveals auxiliary info in a floating layer |

---

## 2. When to Use

| Condition | Use Collapse? |
|-----------|---------------|
| A module is low-priority on the page and should be foldable to free vertical space | YES — 内容全隐藏 |
| The module matters, but its length blocks important content below (e.g. long article, image list) | YES — 内容半隐藏 |
| A long text paragraph exceeds 3 lines and should be truncated with a "See more" link | YES — 文字折叠 |
| FAQ list with many questions, user only cares about a subset | YES — 全隐藏 (FAQ Accordion) |
| Repeated data list (e.g. news, stock rows) where only the first N items matter | YES — `Collapse.List` with `visibleCount` |
| Module groups need unified folding in a Mobile layout | YES — Panel mode |

### MUST

- MUST pick one of the three patterns based on content type (see §5)
- MUST respect the responsive width rules when placing Collapse on Web (see §5d)
- MUST provide a clear expand control (arrow icon or "See more" text) — never hide the collapse state from the user

### SHOULD

- SHOULD use **全隐藏 (Panel)** for grouped/FAQ content where each section stands alone
- SHOULD use **半隐藏 (List + gradient)** only when child cells have unpredictable height (long text, images)
- SHOULD use **文字折叠 (See more / See less)** for long paragraph text, NOT for structured lists
- SHOULD prefer **multi-expand** mode (multiple panels open at once) unless the product explicitly requires accordion behavior

---

## 3. When NOT to Use

| Condition | Use Collapse? | Alternative |
|-----------|---------------|-------------|
| Content is essential and users must see it immediately | NO | Flatten the content, don't fold |
| Switching between peer groups (e.g. tabs by timeframe) | NO | Tab |
| Temporarily showing auxiliary info on hover | NO | Tooltip / Popover |
| Long-form article with a clear reading flow | NO | Scroll / Pagination |
| Critical error messages or alerts | NO | Notice Bar / Notification |
| Form fields that should all be visible during data entry | NO | Flatten the form |

### MUST NOT

- MUST NOT hide primary content (核心功能、主要行动路径) inside Collapse — use Collapse only for secondary/overflow content
- MUST NOT nest Collapse inside Collapse more than one level deep — causes navigation confusion
- MUST NOT hide mandatory form fields behind a collapsed panel
- MUST NOT use Collapse to replace Pagination for long lists that users must scan fully
- MUST NOT use the half-hidden pattern (内容半隐藏) if the hidden child cells are critical for the user's decision — use a dedicated detail page instead

---

## 4. Anatomy

### 4a. 内容全隐藏折叠面板 (Fully Hidden — Collapse.Panel)

```
Collapsed state:
┌──────────────────────────────────────┐
│  Here's how it works              ∨  │
└──────────────────────────────────────┘

Expanded state:
┌──────────────────────────────────────┐
│  Here's how it works              ∧  │
├──────────────────────────────────────┤
│  Full content body goes here.        │
│  Multiple lines supported.           │
└──────────────────────────────────────┘
```

| Part | Required | Description |
|------|----------|-------------|
| Header | MUST | The title line, always visible; acts as the click target |
| Arrow icon | MUST | Right side, `∨` when collapsed and `∧` when expanded |
| Hover zone | MUST | Entire header row is the hit area (`color-hover-5` on hover) |
| Content body | MUST | Hidden when collapsed, revealed when expanded |
| Divider (optional) | Optional | Border line between header and body in expanded state |

### 4b. 内容半隐藏折叠面板 (Half Hidden — Collapse.List + gradient)

```
Collapsed state:
┌──────────────────────────────────────┐
│  WSM  Williams-Sonoma   6.86 +47.10% │
│  WSM  Williams-Sonoma   0.64 +33.33% │
│  WSM  Williams-Sonoma   6.67 +32.89% │  ← last item blurred by gradient
│         ▼  View All  >               │  ← fade + action
└──────────────────────────────────────┘

Expanded state (if the product supports 收起):
┌──────────────────────────────────────┐
│  … all items fully displayed         │
│         ▲  See less                  │
└──────────────────────────────────────┘
```

| Part | Required | Description |
|------|----------|-------------|
| Visible cells | MUST | First N items (default `visibleCount: 3`) fully rendered |
| Gradient mask | MUST | Fades the tail of the last partially-visible cell since child height is unpredictable |
| Expand action | MUST | `View All >` (default) or configurable text — arrow defaults to pointing down |
| Collapse action | Optional | By default, half-hidden pattern does NOT offer 收起 after expanding |
| Expanded collapse arrow | Conditional | Only when the scenario explicitly requires a collapse action — arrow points up and is placed at the bottom of the expanded content, with optional text on its left |
| Scroll anchor | MUST when expanded content is long enough to require scrolling — clicking 收起 MUST scroll the page back to the collapse point |

### 4c. 文字折叠面板 (Text Collapse — "See more / See less")

```
Collapsed (default, fits in 3 lines or fewer visible):
┌────────────────────────────────────────────────┐
│ This legislation would provide pathways for    │
│ cannabis-related businesses to access banking  │
│ services and financing, This legis…  See more  │ ← See more inline after truncation
└────────────────────────────────────────────────┘

Expanded:
┌────────────────────────────────────────────────┐
│ [full paragraph text, all lines shown]         │
│                                                │
│ See less                                       │ ← stays right after the content
└────────────────────────────────────────────────┘
```

| Part | Required | Description |
|------|----------|-------------|
| Text body | MUST | Paragraph text that may exceed 3 lines |
| `See more` link | MUST (when overflowed) | Inline at the end of the truncated text |
| `See less` link | Conditional | Shown after expansion **only when the content module is long enough to warrant a collapse control (屏效考虑)**. When there is enough vertical space, `See less` is NOT shown after expansion |
| Expand behavior | MUST | Click `See more` expands the height of the current container (向下平铺 / push down) — NO floating layer, NO modal |

### 4d. Special Style Row (特殊样式罗列)

Due to technical constraints, when `See more` / `See less` cannot follow the text inline, use **line-break (换行) placement** instead. The action link is placed on its own row directly below the truncated text. This applies to both Web and Mobile.

---

## 5. Variants Overview

### 5a. Pattern Decision — Which of the three to use

| Scenario | Use Pattern | Implementation |
|----------|-------------|----------------|
| Module is low-priority, wants to fold entire module | **全隐藏** | `Collapse` (Panel mode) |
| Module matters but blocks content below, child heights unpredictable | **半隐藏** | `Collapse.List` + custom gradient mask |
| Long paragraph text that exceeds 3 lines | **文字折叠** | `Collapse.Panel` + custom See more / See less rendering |
| Structured repeated list (stocks, news) where only top N matter | **List mode** | `Collapse.List` with `visibleCount` |
| FAQ, where each Q is independent and some users expand multiple | **全隐藏 + multi-expand** | `Collapse` multiple instances |

### 5b. Expansion Mode — Multi-expand vs Single-expand

The Collapse supports two expansion behaviors. **Prefer multi-expand** unless the product explicitly requires accordion.

| Mode | Description | When to Use |
|------|-------------|-------------|
| **Multi-expand (多条展开, PREFERRED)** | User may open multiple Collapse items at the same time | FAQ, help center, grouped settings — users often compare answers side by side |
| **Single-expand (单条展开, accordion)** | Opening a new item automatically collapses the previous | Only when product requirements dictate (e.g. tight space, guided reading flow) |

### 5c. Default State

| Pattern | Default |
|---------|---------|
| 全隐藏 (Panel) | Collapsed (`defaultExpanded: false`) |
| 半隐藏 (List) | Collapsed, first `visibleCount` items visible (default 3) |
| 文字折叠 | Collapsed at 3 lines max |
| FAQ Accordion | Collapsed, OR the first item expanded by default if product requires |

### 5d. Responsive Rules — Web Panel Width (内容区 = 模块容器自身内容区)

The Collapse **width follows the container's inner content area** below it. The "内容区" in these rules means **the module container's own inner content area**, NOT the whole viewport and NOT the main page content area.

| Viewport | Rule |
|----------|------|
| `1410 < 宽度 ≤ 1890` | Panel width = half of the module content area, **max width 756px** |
| `990 < 宽度 ≤ 1410` | Panel width = half of the module content area, **max width 756px** |
| `500 < 宽度 ≤ 990` | If module content area > 756px → panel width = 756px. If ≤ 756px → panel width follows the module content area |
| `宽度 ≤ 500` | Panel width follows the module content area. **Text font size shrinks from 20px to 18px**, max **3 lines**, overflow ellipsis (…) |

Global constraint: when viewport **> 1440**, panel width is capped at half of the viewport as the max width. When viewport **≤ 500**, panel width follows the module content area width.

### 5e. Text Collapse — Text Width Cap

For readability, the **text collapse paragraph width is capped at 756px** on Web. Beyond that width, the text wraps.

### 5f. FAQ Module (Web)

| Setting | Value |
|---------|-------|
| Default state | All collapsed, OR first one expanded (per product) |
| Expansion mode | Multi-expand (preferred) or Single-expand (accordion) |
| Hover zone | **Entire row** (title + right arrow area) |
| Hover background | `color-hover-5` |
| Layout | Stack vertically, each item is a full-width Collapse.Panel |

---

## 6. Content Guidelines

### Panel Header (全隐藏)

- MUST be concise — a clear noun phrase or short question
- MUST fit on a single line; truncate with ellipsis if constrained
- SHOULD describe what's inside so the user can decide whether to expand
- MUST NOT duplicate the content body's first sentence

### Action Labels

| Pattern | Default Expand Label | Default Collapse Label |
|---------|----------------------|-------------------------|
| 全隐藏 (Panel) | — (arrow only) | — (arrow only) |
| 半隐藏 (List) | `View All` (or `View More`) | `See less` (only when 收起 is supported) |
| 文字折叠 | `See more` | `See less` (only when content warrants it) |

- Labels MUST be verbs or verb phrases ("See more", "View All", "Show more details")
- Custom labels via `actionLabel` are allowed and should follow the same voice
- MUST NOT mix `See more` with `View All` on the same page unless the two labels mean semantically different things

### Long Text Inside 文字折叠

- Default visible **3 lines max** before truncation
- Truncation MUST end with ellipsis (`…`)
- `See more` MUST appear inline at the end of the truncated text; fall back to line-break placement when technical limits require it

---

## 7. Layout & Composition

### Placement

- Collapse sits **inline inside a module container** — it does not float
- Collapse header and body occupy the full width of the module's inner content area (subject to the 756px cap on Web)
- Half-hidden (`Collapse.List`) inherits the parent module's padding; the gradient mask starts at the visible cell's lower edge

### Width Rules (see §5d for the full table)

- Respect the responsive rules at all breakpoints
- Never hardcode a width that exceeds **756px** on Web
- On Mobile (`≤ 500`), width always follows the container

### Vertical Spacing

- Gap between stacked Collapse items (FAQ): **default to the module's vertical gap**
- Gap between Collapse header and body when expanded: **8px** minimum
- Gap between half-hidden tail and expand action: **8px**

### FAQ Layout

- Each FAQ question = one `Collapse.Panel`
- Questions are stacked vertically with a consistent divider
- Entire row is the click target — hover state uses `color-hover-5`

---

## 8. Behavior

### Expansion — 全隐藏 (Panel)

- Click anywhere on the header row to toggle
- Arrow rotates from `∨` to `∧` on expand
- Body animates height from 0 to `auto` (height transition)
- Multi-expand is the default behavior; accordion mode is opt-in per product

### Expansion — 半隐藏 (List + gradient)

- Click the `View All` action to expand
- Gradient mask fades out as the full content renders
- **By default the half-hidden pattern does NOT provide a 收起 action** after expansion
- If the product explicitly needs 收起:
  - An upward arrow appears at the bottom of the expanded content, with optional text on its left
  - Clicking it collapses the content back to `visibleCount` items
  - If the expanded content required scrolling to read fully, clicking 收起 MUST scroll the page back to the collapse anchor position

### Expansion — 文字折叠 (See more / See less)

- Click `See more` → the container height grows inline (向下平铺) to fit the full text
- NO floating layer, NO popover, NO modal
- `See less` appears only when the module is long enough that a collapse control is useful for screen efficiency (屏效考虑)
- If there is enough vertical space after expanding (no screen-efficiency concern), `See less` is NOT shown

### Hover (Web only)

- Entire header row is the hover zone
- Background turns `color-hover-5` on hover
- Cursor changes to pointer

### Keyboard Behavior

- The header row MUST be focusable via Tab
- `Enter` or `Space` on a focused header MUST toggle the expansion
- When expanded, focus SHOULD move into the newly revealed content on next Tab

### Click Target

- Mobile: minimum 44×44px hit area
- Web: whole row, no need for 44×44 enforcement on desktop pointer devices

---

## 9. Platform Differences

| Aspect | Web (> 500) | Mobile (≤ 500) |
|--------|-------------|-----------------|
| Header font size | 20px | 18px (auto-shrunk at ≤ 500) |
| Max panel width | 756px | Follows container |
| Max text lines (collapsed) | 3 | 3 |
| Hover interaction | Supported (`color-hover-5`) | N/A (tap only) |
| Click hit area | Whole row | Whole row, ≥ 44×44px |
| FAQ expansion mode | Multi-expand (default) | Multi-expand (default) |
| Scroll anchor on 收起 | Required if expanded content scrolls | Required if expanded content scrolls |

---

## 10. Do / Don't

| | Practice | Reason |
|---|---------|--------|
| DO | Use Collapse for secondary / overflow content only | Primary content must stay visible |
| DO | Pick 全隐藏 for grouped/FAQ sections | Each group is independent and scannable |
| DO | Pick 半隐藏 when child cell height is unpredictable (long text, images) | Gradient mask hides the tail cleanly |
| DO | Pick 文字折叠 for long paragraphs | Preserves reading flow while saving space |
| DO | Default to multi-expand in FAQ | Users often compare answers |
| DO | Cap Web panel width at 756px | Readability + responsive consistency |
| DO | Use `color-hover-5` on the full header row on Web | Consistent hover feedback |
| DO | Show `See less` only when screen efficiency requires it | Avoids redundant controls in short content |
| DO | Add a scroll anchor when 收起 is used on long content | Prevents the user from being stranded mid-scroll |
| DON'T | Nest Collapse more than 1 level deep | Navigation confusion |
| DON'T | Hide primary actions or mandatory form fields inside Collapse | Users may never find them |
| DON'T | Use a floating layer / popover to expand text | Text collapse must be 向下平铺 (inline push) |
| DON'T | Offer 收起 on half-hidden pattern by default | The pattern is designed for one-way expansion |
| DON'T | Exceed 756px panel width on Web | Breaks the responsive system |
| DON'T | Mix `See more`, `View All`, and custom labels inconsistently within the same module | Confuses the user |
| DON'T | Use Collapse to replace Pagination for long lists users must scan fully | Pagination is the correct pattern |
| DON'T | Collapse content that must be seen for decision-making | Defeats the purpose of the page |

---

## 11. Decision Table

### Which Collapse Pattern

| Need | Pattern | Implementation |
|------|---------|----------------|
| Fold an entire module (FAQ, secondary section) | **全隐藏** | `Collapse` (Panel) |
| Fold a long list of structured rows, show top N | **List mode** | `Collapse.List` with `visibleCount` |
| List with unpredictable child height (text + images) | **半隐藏** | `Collapse.List` + gradient mask |
| Long paragraph text | **文字折叠** | `Collapse.Panel` + See more / See less |
| Accordion behavior (one-at-a-time) | **全隐藏 + Single-expand** | Product-controlled state logic |

### Expansion Mode

| Product context | Mode |
|-----------------|------|
| FAQ, help center, settings groups | **Multi-expand** (preferred) |
| Guided reading flow, tight vertical space | Single-expand (accordion) |

### Responsive Width (Web)

| Viewport | Panel width rule |
|----------|-------------------|
| 1410 < 宽度 ≤ 1890 | Module content area / 2, max 756px |
| 990 < 宽度 ≤ 1410 | Module content area / 2, max 756px |
| 500 < 宽度 ≤ 990 | If module content > 756px → 756px; else follow content |
| ≤ 500 | Follow module content, font 20 → 18, max 3 lines |

### `See less` Visibility (文字折叠)

| Content length | After expand |
|----------------|--------------|
| Module is long enough that screen efficiency matters | Show `See less` below content |
| There is enough vertical space; collapse control is not useful | Do NOT show `See less` |

### Half-Hidden 收起 Action

| Product need | 收起 action |
|--------------|-------------|
| Default (informational list) | NOT provided |
| Expanded content is long and users may want to collapse again | Provide upward arrow at the bottom; add scroll anchor |

### FAQ Hover

| State | Background |
|-------|-----------|
| Default | Transparent |
| Hover on any part of the row | `color-hover-5` |
| Expanded (body shown) | Same as default, hover still applies to header row |

---

## 12. Related Documents

- `ainvest-design-system/references/components/collapse.md` — Component style spec (Panel + List API, code examples)
- `ainvest-design-system/references/rules/NoticeBar.md` — Notice Bar usage guideline (for related inline banner patterns)
- `ainvest-design-system/references/rules/Tab.md` — Tab usage guideline (peer group switching, not folding)
- `ainvest-design-system/references/rules/Button.md` — Button usage (for `View All` / `See more` action styling)
- `ainvest-design-system/SKILL.md` — Unified skill file
- `ainvest-design-system/references/tokens/color.md` — Color tokens (including `color-hover-5`)
- `ainvest-design-system/references/tokens/spacing.md` — Spacing tokens
- `ainvest-design-system/references/tokens/typography.md` — Typography tokens (20px / 18px text sizing)
