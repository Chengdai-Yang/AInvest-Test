# logos/

Product-owned logos and branded image assets. **Not for financial ticker logos** — those come from CDN (see `references/rules/image-asset-usage.md §6.1`).

---

## What belongs here

| Type | Example files | Purpose |
|---|---|---|
| Ainvest brand logo | `ainvest-logo.svg` | Nav, footer, splash, any place the product name appears |
| AIme character | `aime-animated.png`, `aime-static.png` | AI assistant avatar (animated for loading, static for inline) |
| Stock logo local fallbacks | `stocks/{TICKER}.png` | Tickers that 404 on CDN but still need a real logo (rare) |

**Do NOT add here:** CDN-served ticker logos (AAPL, NVDA, BTC etc.), politician avatars (use `assets/avatars/politicians/`), UI functional icons (use `assets/icons/`).

---

## Loading strategy — three-tier fallback

For any ticker / crypto / ETF logo, consumers MUST follow this order:

1. **CDN first** — `https://cdn.ainvest.com/icon/{market}/{SYMBOL}.png` (see `image-asset-usage.md §6.1` for market-specific paths)
2. **Local fallback** — if CDN 404s and a local `stocks/{SYMBOL}.png` exists, use it
3. **Initials fallback** — if neither exists, render 1–2 uppercase letters on `color.bg.weak` in `color.text.tertiary` (see `image-asset-usage.md §1` MUST rules)

For brand / AIme assets, always use the local file directly — no CDN fallback needed.

---

## Naming convention

| Asset | Rule |
|---|---|
| Stock local fallback | `stocks/{TICKER}.png` — **uppercase, case-sensitive** (`AAPL.png` ≠ `aapl.png`) |
| Brand logo | `ainvest-logo.svg` — single canonical file, recolored via CSS filter (see `image-asset-usage.md §5.1`) |
| AIme | `aime-animated.png` / `aime-static.png` — no version suffix, replace in place |

**SVG warning:** Do not add `.svg` for ticker logos. The Ainvest CDN serves `.svg` as plain colored blocks, not real logos — always use `.png` (`image-asset-usage.md §6.1 CRITICAL`).

---

## Adding a new asset

1. Put the file in this directory following the naming convention above
2. If it's a brand / AIme asset, register it in `image-asset-usage.md §5`
3. If it's a stock local fallback, no registration needed — the three-tier loader picks it up automatically
4. Commit — no build step required

---

## Related rules

- `references/rules/image-asset-usage.md` — full image rendering rules (circular mask, object-fit, fallback behavior)
- `references/tokens/color.md` — `color.bg.weak`, `color.text.tertiary` tokens used by initials fallback
