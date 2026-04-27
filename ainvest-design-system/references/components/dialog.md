# Guidelines — Dialog (弹窗 / Modal Dialog)

## 0. Document Role

This document covers **when and how to use** the Dialog component. Visual details (spacing, typography, border-radius, scroll behavior, text alignment) are handled automatically by the component — this doc focuses on decisions a developer or AI must make explicitly.

- Component import: `@ainvest/dialog`,`@ainvest/marketing-dialog`
- Related: `底部弹窗 (Drawer/Bottom Sheet)`, `行动列表 (Action Sheet)`, `Toast`, `提示框 (Alert)`

---

## 1. Definition

A **Dialog** is a modal window that blocks all background interaction until dismissed. Use it only for content or actions that require the user's immediate attention.

---

## 2. When to Use

Use a Dialog when **all** of the following are true:

- The user must acknowledge or act before continuing.
- The content is critical, time-sensitive, or involves an irreversible action (e.g., confirm deletion, payment, permission request).
- The message cannot be handled by a non-modal pattern (Toast, inline error, Banner).
- No more than **3 action buttons** are needed.

---

## 3. When NOT to Use

| Situation | Use Instead |
|---|---|
| Simple status feedback that needs no action | Toast |
| Large forms or multi-step flows | Drawer (底部弹窗) or a new page |
| Passive informational notices | Banner / Notification |

> If the user does not *need* to respond right now, do not use a Dialog.

---

## 4. Anatomy

### Required
- **`DialogBody` + `DialogText`** — content body is always required.
- **At least one button in `DialogFooter`** — always required.

### Optional
- **`DialogHeader` + `DialogTitle`** — add when body text exceeds 3 lines, or for Marketing Dialogs. The close icon is hidden by default on `variant="app"`.
- **Secondary / Tertiary buttons** — add based on action hierarchy (see §5).
- **Image area** — Marketing Dialog only, placed above title and content.

---

## 5. Variants & Configuration

### `variant` — Choose Platform Style

| Value | When to Use |
|---|---|
| `"web"` (default) | Desktop/web — includes close (×) button, supports `size` and `fullscreen` props |
| `"app"` | Mobile — centered layout, no close button, full-width buttons |

### `size` — Web Only

| Value | Width | When to Use |
|---|---|---|
| `"sm"` | `420px` | Short confirmations, simple yes/no, rename inputs |
| `"md"` (default) | `630px` | Most standard dialogs |
| `"lg"` | `850px` | Rich content: long text, option lists, dual-column layouts |

### `fullscreen` — Web Only

| Value | Behavior |
|---|---|
| `"never"` (default) | Always a centered floating card |
| `"sm"` | Centered card on large screens → **full-screen on small screens (≤ 500px)** |

Use `fullscreen="sm"` for **functional dialogs** that require sustained interaction (e.g., Add Symbol, Add Indicators, Settings). Do **not** use for simple text confirmations or marketing dialogs.

### `preventInteractOutside` — on `DialogContent`

Set to `true` when the user **must not** dismiss the dialog by clicking the overlay (e.g., mid-transaction flow, mandatory onboarding step). Defaults to `false`.

---

## 6. Content Guidelines

### Title (`DialogTitle`)
- **Required** when body text exceeds 3 lines.
- **Required** in all Marketing Dialogs.
- Otherwise optional. Keep to a short noun phrase or clear imperative.

### Body Text (`DialogText`)
- Keep copy concise — users are interrupted.
- Do not use bullet lists or sub-headings inside the dialog body.
- Text alignment (center ↔ left) adjusts automatically based on line count — no implementation needed.

### Button Labels
- Use clear action verbs: "Confirm", "Cancel", "Delete", "Learn More".
- Avoid vague labels like "OK" or "Yes" for destructive or irreversible actions.
- The primary button label should reinforce the dialog's core intent.

### Image (Marketing Dialog only)
- Recommended aspect ratio: `16:9` (adjustable per scene).
- Do not add images to standard dialogs.
- **Web marketing dialog:** image appears on the **left side**, content on the right.
- **App marketing dialog:** image appears at the **top**, content below.

### Marketing Dialog Content Limits (App)
- Title: max 2 lines, ≤ 45 characters.
- Body: max 3 lines, ≤ 50 characters.

---

## 7. Layout & Composition

The component handles all spacing, sizing, and responsive layout automatically. The only layout decisions left to the implementer are:

- **Web:** Which `size` preset to use (`sm` / `md` / `lg`).
- **Web:** Whether to enable `fullscreen="sm"` for functional dialogs.
- **Button arrangement in `DialogFooter`:** horizontal (default) or vertical. See §8.

---

## 8. Behavior

### Closing
- **Web standard dialog:** dismissed via × button, ESC key, or overlay click (unless `preventInteractOutside` is set).
- **App standard dialog:** dismissed only via a button tap — no close icon, no overlay dismiss.
- **Marketing dialog (both platforms):** always has a × close button.

### Scrollable Content
Handled automatically. When content exceeds max height, `DialogBody` scrolls internally while `DialogFooter` stays sticky.

### Button Layout in `DialogFooter`

Button layout is not auto-managed — the implementer chooses:

| Layout | When to Use | Implementation |
|---|---|---|
| Horizontal (default) | 2 buttons with short labels | Default `DialogFooter` — add `flex-1` to each button |
| Vertical stack | 2–3 buttons, or labels too long for side-by-side | `<DialogFooter className="flex-col gap-2">` |
| Single full-width | 1 primary action only | `<Button className="flex-1">` or `className="w-full"` |

**Button hierarchy rule:** primary (filled) button goes **last** in horizontal layout, **first** in vertical layout.

---

## 9. Platform Differences

| Aspect | App (`variant="app"`) | Web (`variant="web"`) |
|---|---|---|
| Close button | None by default | Always shown (×), top-right |
| Dismiss on overlay click | No | Yes (unless `preventInteractOutside`) |
| Size control | Not applicable | `size="sm/md/lg"` |
| Full-screen mode | Not applicable | `fullscreen="sm"` |
| Button width | Full-width by default | Side-by-side by default |
| Image position (marketing) | Top of card | Left side of card |

---

## 10. Do / Don't

| ✅ Do | ❌ Don't | Reason |
|---|---|---|
| Use Dialog only when the user must act immediately | Use for passive notifications or status feedback | Dialogs interrupt — overuse erodes trust |
| Add `DialogTitle` when body text exceeds 3 lines | Leave long text without a title | Long text without a title has no visual anchor |
| Use Marketing Dialog variant for promotions | Add images to standard dialogs | Images dilute urgency in standard dialogs |
| Use `16:9` image ratio in Marketing Dialogs | Use arbitrary image ratios | Inconsistent ratios break the layout |
| Use clear action verbs on buttons | Use "OK" / "Yes" for destructive actions | Vague labels increase error rates |
| Use `fullscreen="sm"` for functional/operation dialogs | Use `fullscreen="sm"` for simple confirmations or marketing dialogs | Full-screen is for sustained interaction, not brief prompts |
| Set `preventInteractOutside` only for mandatory flows | Apply it broadly as a default | Unnecessarily trapping users creates frustration |
| Limit to a maximum of 3 buttons | Add 4 or more buttons | More than 3 actions belong in an Action Sheet or new page |
| Place primary button last (horizontal) or first (vertical) | Mix button order between layouts | Inconsistent placement breaks user muscle memory |

---

## 11. Decision Table

### App (`variant="app"`)

| Scenario | Title | Buttons | Notes |
|---|---|---|---|
| Confirm irreversible action | Optional | 1 primary (full-width) | `showCloseIcon={false}` on `DialogHeader` |
| Main action + one alternative | Optional | 2 horizontal | |
| Button labels too long for horizontal | Optional | 2 vertical | `DialogFooter className="flex-col gap-2"` |
| Main action + 2 lower-priority alternatives | Optional | 3 vertical (filled → outlined → ghost) | |
| Body text > 3 lines | **Required** | Any config above | Component handles left-align automatically |
| Promotional / reward / campaign | **Required** | Primary (+ optional secondary) | Marketing Dialog; image required (16:9) |
| Simple status feedback | ❌ | — | Use Toast instead |
| Selectable option list | ❌ | — | Use Action Sheet instead |

### Web (`variant="web"`)

| Scenario | Size | `fullscreen` | Notes |
|---|---|---|---|
| Simple confirmation / destructive action | `sm` | `"never"` | |
| Text input (rename, entry) | `sm` | `"never"` | |
| Standard confirmation with more content | `md` | `"never"` | |
| Single-column functional list (e.g., Add Symbol) | `md` or `lg` | `"sm"` | Full-screen on mobile |
| Dual-column operation (e.g., Add Indicators) | `lg` | `"sm"` | Dual-col auto-collapses; full-screen on mobile |
| Multi-panel / settings-style dialog | `lg` | `"sm"` | Full-screen on mobile |
| Campaign / news / editorial promotion | Marketing Dialog | `"never"` | Image on left, content right |
| Reward / gift / immersive brand promotion | Pure Marketing Dialog | `"never"` | Full branded card |

---

## 12. Related Documents

- `@ainvest/dialog` — Component source and props reference
- `/components/drawer.md` — Bottom Sheet (底部弹窗)
- `/components/toast.md` — Toast (消息框)
- `/design-system/SKILL.md` — AInvest brand design tokens
