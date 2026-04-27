# Guidelines — segment-tabs

> **Component Spec:** `ainvest-design-system/references/components/segment-tabs.md`

---

## 0. Document Role

This document covers **when and how to use** the segment-tabs (分段标签页) component. It focuses on decisions a designer must make explicitly — visual details handled automatically by the component are omitted.

- Component import: `@ainvest/segment-tabs`

---

## 1. Definition

segment-tabs is a **module-level primary switcher** rendered as a single capsule-shaped segmented control. All options live inside one rounded container, with the active option highlighted by a filled pill.

Typical usage is inside a self-contained module (chart card, data widget, detail card) where the user switches between **parallel slices of the same data set** — most commonly **timeframes** (1D / 5D / 1M / 3M / 6M / 1Y / 5Y / All), but also indices / markets / units / other dense mutually-exclusive filters.

Compared with other tab sub-types:

| Sub-type | Purpose |
|----------|---------|
| line-tabs | Page-level primary navigation — independent sibling views |
| pill-tabs | Page-level secondary / module-level switching with separate capsule items |
| **segment-tabs** | Module-level dense segmented control inside a single bordered capsule |

---

## 2. When to Use

| Condition | Use segment-tabs? |
|-----------|-------------------|
| Chart timeframe switcher (1D / 5D / 1M / 3M / 6M / 1Y / 5Y / All) | YES |
| Dense mutually-exclusive filters over the same data (e.g. Indices / Stocks / Crypto / Futures / Forex / Bonds / ETFs inside a market widget) | YES |
| Module-level primary switcher where all options are short labels and must be visible at a glance | YES |
| A segmented control inside a card / chart / widget | YES |

### MUST

- MUST use segment-tabs only at **module level** (inside a card / widget), not as page-level navigation
- MUST keep exactly one option selected at all times (single-select)
- MUST keep option labels **short and uniform** in length (1–3 characters or a single word is typical)
- MUST keep the whole control on a single line — never wrap

### SHOULD

- SHOULD use segment-tabs when options are parallel slices of the **same** metric (e.g. timeframes of one chart)
- SHOULD use this component when the number of options is small to medium and all labels are short

---

## 3. When NOT to Use

| Condition | Use Instead |
|-----------|-------------|
| Page-level primary navigation between independent sections | line-tabs |
| Page-level secondary or module-level switching with longer labels | pill-tabs |
| Options lead to independent page sections (not slices of one data set) | line-tabs / pill-tabs |
| Action trigger (submit, save, delete) | Button |
| Multi-select filtering | Checkbox group / Chip group |
| Only one option exists | Remove the control entirely |

### MUST NOT

- MUST NOT use segment-tabs for actions — use Button
- MUST NOT use segment-tabs when only one option is available (≥ 2 options required)
- MUST NOT allow zero options to be selected
- MUST NOT mix long-form labels with short labels inside one segment-tabs
- MUST NOT use segment-tabs as page-level primary navigation

---

## 4. Anatomy

```
╭──────────┬──────┬──────┬──────┬──────┬──────┬──────┬──────╮
│ 1 day ▼  │  5D  │  1M  │  3M  │  6M  │  1Y  │  5Y  │  All │
│  0.24%   │ 0.68%│ 3.24%│ 8.14%│ 5.94%│ 2.40%│44.12%│ 393… │
╰──────────┴──────┴──────┴──────┴──────┴──────┴──────┴──────╯
   ▲ active pill
```

| Part | Required | Description |
|------|----------|-------------|
| Outer Capsule | MUST | Single rounded container enclosing all options |
| Option Label | MUST | Short text identifying each segment |
| Option Sub-value | Optional | A secondary value (e.g. % change) rendered under the label on the same option (see §5b) |
| Active Pill | MUST | Filled background marking the currently selected option |
| Dropdown Caret (`▼`) on an option | Optional | Signals that the option has an additional sub-picker (e.g. "1 day" opens a finer-grain picker) |
| Overflow "More" Entry | Conditional | Appears at the right end of the capsule when options do not fit (Web, §6) |

---

## 5. Variants Overview

### 5a. Option Content Layouts

| Sub-variant | Description | When |
|-------------|-------------|------|
| **Label only** | Each option shows a single short label | Default — use when no comparable per-option value is available |
| **Label + sub-value** | Each option shows a label on top and a secondary value below (e.g. % change, count) | Use when the sub-value helps the user choose the segment (e.g. chart timeframe showing the return for each range) |

Rules:

- MUST use **one** layout consistently across all options in a single segment-tabs — do not mix "label only" and "label + sub-value"
- Sub-values MUST be of the **same type and format** across options (e.g. all percentages with the same decimals and sign style)

### 5b. Option with Dropdown Caret

A single option MAY carry a dropdown caret (`▼`) indicating that clicking it opens a finer-grain picker (e.g. "1 day" expands to intraday intervals).

- MUST be limited to **one** option per segment-tabs (typically the first / shortest timeframe)
- The caret is **part of the option**, not a separate action — clicking the option still selects it
- If no finer-grain picker is needed, do not add the caret

---

## 6. Behavior

### Selection

- Exactly **one** option must be selected at all times
- Clicking an unselected option switches the active pill and the content immediately
- Clicking the already-selected option is a no-op (unless it has a dropdown caret, in which case it opens the sub-picker)

### Overflow — when options do not fit

The component handles overflow automatically, with **two strategies** depending on content volume:

#### 6a. Few options, does not fit — **truncate**

When the option count is small but still exceeds the available width, the capsule is **truncated at its right edge**. The last visible option is cut off visually; no overflow entry is added.

- Use this strategy when shrinking the container further would be acceptable (e.g. the card has a hard width and the last timeframe is rarely used)
- Designers SHOULD prefer reducing the option count before relying on truncation

#### 6b. Many options, does not fit (Web) — **shrink then "More"**

When the option count is larger and the row does not fit, the component applies a two-step adaptation:

1. **Shrink first** — reduce each option's left / right padding down to the tab's minimum width value
2. **Collapse into "More"** — if options still do not fit, hide the overflowed options behind a **"More ▼"** entry at the right end of the capsule; clicking "More" reveals the hidden options

Designers only need to decide **which strategy is acceptable for the module**. Do not redesign the overflow visual — the component provides it.

### Keyboard

- `ArrowLeft` / `ArrowRight`: move focus between options
- `Home` / `End`: move to first / last option
- `Tab`: move focus out of the control

---

## 7. Configurable Options

The following decisions should be made explicitly by the designer; other visual details are handled by the component.

| Option | Purpose | Typical Values |
|--------|---------|----------------|
| `defaultValue` / `value` | Which option is active on load / controlled value | option `value` string |
| Option content layout | Label only, or label + sub-value | `"label"` / `"label+value"` (consistent across all options) |
| Dropdown caret on an option | Whether a single option opens a finer-grain picker | present on ≤ 1 option / absent |
| Overflow strategy | Which behavior applies when the row does not fit | `"truncate"` (few options, §6a) / `"shrink-then-more"` (many options Web, §6b) |

Designers MUST decide explicitly:

- Which **option content layout** (§5a) applies, and ensure it is consistent across all options
- Whether any option needs a **dropdown caret** (§5b), and if so, which one
- Which **overflow strategy** (§6a vs §6b) is acceptable given the expected option count and container width

---

## 8. Do / Don't

| | Practice | Reason |
|---|---------|--------|
| DO | Use segment-tabs for chart timeframes and other dense module-level slicers | Matches the component's intended scenario |
| DO | Keep all labels short and uniform in length | The segmented capsule depends on visual rhythm |
| DO | Use one option layout consistently (all label-only, or all label+sub-value) | Mixing layouts breaks the segmented rhythm |
| DO | Place the dropdown caret on at most one option | Two carets create ambiguous hierarchy |
| DO | Let the component handle overflow (truncate for few, shrink-then-More for many on Web) | Overflow visuals are authoritative |
| DON'T | Use segment-tabs as page-level primary navigation | Use line-tabs — segment-tabs is module-scoped |
| DON'T | Mix short labels with long-form labels inside one segment-tabs | Causes uneven widths and forced wrapping |
| DON'T | Wrap segment-tabs onto two lines | The control must stay on a single line |
| DON'T | Use segment-tabs when options are not parallel slices of the same data | Use pill-tabs or line-tabs instead |
| DON'T | Redesign the "More" overflow visual per page | The component's default behavior is authoritative |
| DON'T | Allow zero selected options | segment-tabs is single-select — always one active |
