# Design System — Shadow Tokens

All UI MUST follow this shadow system.

---

# 0. Core Principles

- MUST use semantic shadow tokens
- MUST NOT define custom shadow values
- MUST reflect elevation hierarchy
- MUST use minimal shadow for clarity

---

## ⚠️ Trigger Conditions (CRITICAL)

**Shadow applies ONLY in two situations:**

1. Floating elements triggered by interaction (dropdowns, popovers, modals, dialogs)
2. Web-only interactive card `:hover` state (appears only on hover; default state has no shadow)

**Static elements MUST NOT have shadow** — including static cards, buttons, inputs, navbars, and modules. Use borders / dividers / bg tokens to express structure instead.

### Platform split

- **App cards have no hover state** → App cards MUST NOT have shadow in any state
- `shadow.lg` is Web-only; App does not use large shadows
- App `shadow.md` is reserved for explicit-need cases only
- Web `shadow.md` vs App `shadow.md` use different values — MUST NOT mix

### Hover interaction

- On hover, ONLY shadow changes (Web interactive card only) — MUST NOT apply `transform`, translate, or scale

---

# 1. Shadow Tokens

## shadow.sm

### Web
- value: 0px 0px 8px rgba(0,0,0,0.12)

### App
- value: 0px 0px 8px rgba(0,0,0,0.12)

---

## shadow.md

### Web
- value: 0px 2px 8px rgba(0,0,0,0.15)

### App
- value: 0px 4px 12px rgba(0,0,0,0.12)

---

## shadow.lg

### Web
- value: 0px 8px 32px rgba(0,0,0,0.20)

### App
- value: 0px 0px 20px rgba(0,0,0,0.12)

---

# 2. Usage Definition

## shadow.sm (small)

- buttons
- icons
- lightweight elements
- subtle elevation

---

## shadow.md (medium)

- cards
- dropdown menus
- floating panels

---

## shadow.lg (large)

- modal
- important surfaces
- attention-focused components

---

# 3. Rules

### MUST

- MUST use shadow.md as default elevation
- MUST use shadow.sm for lightweight UI
- MUST use shadow.lg only for high emphasis
- MUST follow component hierarchy

---

### MUST NOT

- MUST NOT overuse shadow
- MUST NOT mix multiple shadow levels in one component
- MUST NOT use lg for regular UI
- MUST NOT create custom shadow values

---

# 4. Platform Rule（非常关键）

- Web: stronger shadow hierarchy (depth perception)
- App: softer shadow (lightweight feel)

---

# 5. Component Mapping

### Button / Icon
- shadow.sm

---

### Card
- shadow.md

---

### Dropdown / Popover
- shadow.md

---

### Modal
- shadow.lg

---

# 6. AI Rules

### Mapping Logic

- IF component = button → shadow.sm
- IF component = icon → shadow.sm
- IF component = card → shadow.md
- IF component = dropdown → shadow.md
- IF component = modal → shadow.lg

---

### Restrictions

- DO NOT use multiple shadow levels in one component
- DO NOT use shadow for non-elevated elements

---

### FAILURE

If violated → regenerate UI