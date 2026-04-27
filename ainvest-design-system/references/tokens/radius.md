# Design System — Radius Tokens

All UI MUST follow this radius system.

---

# 0. Core Principles

- MUST use semantic radius tokens
- MUST NOT define arbitrary radius values
- MUST keep consistency within a component
- MUST follow component-level rules

---

## ⚠️ Component Isolation (CRITICAL)

**Global radius tokens MUST NOT override the intrinsic radius of control components (buttons, tabs, inputs, tags).**

- Buttons, tabs, inputs, and tags have their own fixed radius defined in their component docs (`references/components/`)
- When using a component, ALWAYS check the component's own doc first for its radius rule
- Global radius tokens apply ONLY to cards, containers, and elements without a component-level radius rule

---

# 1. Radius Tokens

### radius.sm
- value: 6

### radius.md
- value: 16

### radius.lg
- value: 24

### radius.pill
- value: 9999px

### radius.full
- value: 50%

---

# 2. Usage Definition

## radius.sm (6)

- small elements
- tags / chips
- segmented controls
- compact UI components

---

## radius.md (16)

- default radius
- cards
- dropdowns
- containers

---

## radius.lg (24)

- large cards
- floating panels
- onboarding / marketing cards

---

## radius.pill (9999px)

- **Full Rounded** buttons (capsule: straight top/bottom, semicircular ends)
- tabs and pill-shaped nav / segmented controls
- any **horizontal pill** where width is not fixed to height

## radius.full (50%)

- **square** boxes only (width === height): avatars, circular icon buttons, dots
- yields a **true circle** only when the box is square

### Web implementation (critical)

- On a **rectangle** wider than it is tall, `border-radius: 50%` does **not** produce a capsule; the outline becomes an **ellipse** (curved top and bottom). That is **not** the intended Full Rounded button shape.
- For capsule UI, MUST use **`radius.pill`** (e.g. `9999px`), not `radius.full` on non-square elements.

---

# 3. Rules

### MUST

- MUST use radius.md as default
- MUST use radius.sm for compact elements
- MUST use radius.lg only for large containers
- MUST use **radius.pill** for pill-shaped buttons, tabs, and horizontal pills
- MUST use **radius.full** only on **square** elements that should read as a circle

---

### MUST NOT

- MUST NOT mix multiple radius in one component
- MUST NOT use lg for small elements
- MUST NOT use arbitrary radius values
- MUST NOT apply `radius.full` (50%) as the sole rounding on wide rectangular buttons or tabs — use `radius.pill` instead

---

# 4. Scaling Rule（非常关键）

When a component is scaled:

- MUST scale radius proportionally
- MUST round the final value for visual alignment

---

# 5. Component Mapping（给AI用）

### Button

- Full Rounded (default) → **radius.pill**

---

### Tab

- default → **radius.pill**

---

### Card

- default → radius.md
- large card → radius.lg

---

### Tag / Chip

- default → radius.sm

---

# 6. AI Rules

### Mapping Logic

- IF component = button (Full Rounded) → use **radius.pill**
- IF component = tab / pill nav → use **radius.pill**
- IF component = square avatar / circular dot / square icon mask → use **radius.full**
- IF component = card → use radius.md
- IF component = large container → use radius.lg
- IF component = tag → use radius.sm

---

### Restrictions

- DO NOT use multiple radius in one component
- DO NOT use random radius

---

### FAILURE

If violated → regenerate UI