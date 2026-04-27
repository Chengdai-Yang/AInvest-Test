# Image & Asset Usage Rules

**Category:** Asset Presentation  
**Scope:** All images used in UI — stock ticker logos, politician avatars, news thumbnails, company logos, any `<img>` element rendered inside a component

---

## 1. Stock / Ticker Logos

### Rule — Always circular

Stock ticker logos MUST always be displayed as a circle, regardless of the component they appear in (carousel cards, table rows, list rows, tooltips, etc.).

### MUST

- MUST apply `border-radius: 50%` **and** `overflow: hidden` to the logo **container** element
- MUST apply `object-fit: cover` to the `<img>` so the image fills the circle without distortion or letterboxing
- MUST provide a neutral fallback background (`color.bg.weak`) on the container, visible when the image fails to load or is still loading
- MUST render an **initials fallback** when the CDN image fails to load (404 / error): hide the `<img>`, show the ticker symbol's first 1–2 characters as uppercase text centered inside the circle, using `color.text.tertiary` on a `color.bg.weak` background

### MUST NOT

- MUST NOT display ticker logos as squares or rounded rectangles
- MUST NOT apply `border-radius: 50%` only to the `<img>` tag without `overflow: hidden` on the container — the container is what actually clips the image
- MUST NOT use `border-radius` values other than `50%` for ticker logos (e.g. `4px`, `8px`)
- MUST NOT leave an empty circle when a CDN image 404s — always show initials fallback

### CSS Reference

```css
.ticker-logo {
  border-radius: 50%;
  overflow: hidden;           /* clips the image to the circle */
  background: var(--color-bg-weak);
}
.ticker-logo img {
  object-fit: cover;
  display: block;
  width: 100%;
  height: 100%;
}
```

### Rationale

Ticker logos sourced from CDN are square PNGs with varying internal padding and background colors. Circular masking creates visual consistency across all tickers regardless of the source image format, and aligns with the rounded visual language of the Ainvest design system.

---

## 2. Politician / Person Avatars

### Rule — Always circular

Person avatars (politicians, analysts, authors) MUST always be displayed as a circle.

### MUST

- MUST apply `border-radius: 50%` and `overflow: hidden` to the avatar container
- MUST apply `object-fit: cover` so the face is not stretched
- MUST use local asset files for politician avatars when available (see §6.3 below)
- MUST render initials fallback (colored circle + 2-letter initials, party color) when no image is available

### MUST NOT

- MUST NOT display avatars as squares or rounded rectangles
- MUST NOT use CDN politician avatar URLs — they are not reliably available

---

## 3. News / Article Thumbnails

### Rule — Rounded rectangle

News thumbnails use `border-radius: var(--radius-sm)` (not circular).

### MUST

- MUST use `border-radius: var(--radius-sm)` (6px)
- MUST use `object-fit: cover` to fill the container without distortion

---

## 4. General Image Rules

- MUST always set explicit `width` and `height` on `<img>` elements to prevent layout shift
- MUST use `display: block` on `<img>` to eliminate baseline gap
- MUST provide a fallback background color on the container for loading states

---

## 5. Brand Logo & AIme Assets

### 5.1 Ainvest Brand Logo

**Local file:** `assets/logos/ainvest-logo.svg`

The Ainvest logo SVG uses hardcoded `fill="#165DFF"` (brand blue) for all paths. It cannot be recolored via CSS `color` property. Use CSS `filter` to produce the white variant for dark backgrounds.

| Variant | Usage | CSS |
|---|---|---|
| 标准版 Brand Blue | Light/white backgrounds, standard nav | No filter needed — renders as `#165DFF` by default |
| 反白版 White | Dark backgrounds, dark nav, dark overlays | `filter: brightness(0) invert(1)` — converts all fills to white |

```html
<!-- Light background (brand blue — default) -->
<a href="/" class="navbar-logo">
  <!-- inline ainvest-logo.svg content here -->
</a>

<!-- Dark background (white via filter) -->
<a href="/" class="navbar-logo" style="filter: brightness(0) invert(1);">
  <!-- inline ainvest-logo.svg content here -->
</a>
```

### MUST

- MUST use the inline SVG directly — do not use `<img src="...svg">` as filter won't apply cross-origin
- MUST use `filter: brightness(0) invert(1)` for white variant on dark backgrounds
- MUST NOT hardcode a different fill color in the SVG — use filter only
- Default (no filter) renders as brand blue `#165DFF` — correct for light/white page backgrounds

---

### 5.2 AIme Character Assets

**Local files:** `assets/logos/`

| File | Type | Usage |
|---|---|---|
| `aime-animated.png` | APNG (animated) | Loading states, chat entry, onboarding splash |
| `aime-static.png` | PNG (static) | Chat thinking indicator, inline avatar, small UI contexts |

**Source URLs (for reference / re-download):**
- Animated: `https://lh-prod-oper-pub-opercenter.s3.amazonaws.com/activity_ainvest_com/apngb-animated.png`
- Static: `https://cdn.ainvest.com/frontResources/s/iwc-aime-screener/static/1.29.2/thinking-loading-aime-2756644e.png`

### MUST

- MUST use the animated version for interactive/loading contexts and the static version for compact UI placements (avatars, list rows, tooltips)
- MUST display AIme assets as a circle (same rules as §2 Avatars: `border-radius: 50%` + `overflow: hidden` + `object-fit: cover`)
- MUST NOT stretch or distort AIme assets — always use `object-fit: cover`

---

## 6. CDN Asset Registry

**CDN Root:** `https://cdn.ainvest.com/`

All public images are hosted on CDN via AWS CloudFront. Paths are case-sensitive.

### 6.1 Financial Data Assets (cache 30d)

These assets are data-driven — they change with market data and must not be hardcoded.

| Category | Path | Format | Example URL |
|---|---|---|---|
| 股票 Stock | `icon/us/{SYMBOL}` | **`.png` only** | `https://cdn.ainvest.com/icon/us/AAPL.png` |
| ETF | `icon/us/etf/{SYMBOL}` | **`.png` only** | `https://cdn.ainvest.com/icon/us/etf/AAAU.png` |
| 债券 Bond | `icon/us/bond/{SYMBOL}` | `.png` | `https://cdn.ainvest.com/icon/us/bond/GECCI.png` |
| 板块 Sector (icon) | `icon/us_block/icon/{ID}` | `.png` | `https://cdn.ainvest.com/icon/us_block/icon/861062.png` |
| 板块背景图 Sector (bg) | `icon/us_block/background/{ID}` | `.png` | `https://cdn.ainvest.com/icon/us_block/background/861062.png` |
| 数字货币 Crypto | `icon/crypto/{SYMBOL}` | **`.png` only** | `https://cdn.ainvest.com/icon/crypto/BTC.png` |
| 国旗 National Flag | `icon/national-flag/{CODE}` | `.png` | `https://cdn.ainvest.com/icon/national-flag/US.png` |
| 券商 Logo Broker | `icon/brokers/{NAME}` | `.png` | `https://cdn.ainvest.com/icon/brokers/Alpaca.png` |

> **Symbol naming**: case-sensitive, match the exact ticker/code from the data source.  
> **Country code**: ISO 3166-1 alpha-2, ~80 countries covered.

> **CRITICAL — SVG NOT SUPPORTED for financial icons**: The CDN serves `.svg` files for stock/crypto/ETF logos but the SVG content is a plain colored block, NOT the actual logo graphic. Always use `.png` for all financial data assets. Never use `.svg` for ticker logos, crypto icons, or ETF logos.

### 6.2 Product / UI Assets (cache 180d)

These assets are designed and managed by the design/product team. They change infrequently.

| Category | Path | Format | Notes |
|---|---|---|---|
| 付费工具图标 Premium Tools | `icon/premium/{filename}` | `.png` | e.g. `icon_aime_basic.png`, `icon_peak_seeker.png` |
| 免费工具图标 Free Tools | `icon/free/{filename}` | `.png` | e.g. `icon_7_days_free_trail.png` |
| 货币符号 Currency | `icon/currency/{CODE}` | `.png` or `.svg` | e.g. `https://cdn.ainvest.com/icon/currency/CNY.svg` |
| App 图标 App Icons | `icon/app/{key}` | TBD | 路径已规划，待上传 |
| 大师持仓 Guru | `icon/guru/{id}` | TBD | 待上传 |
| AI 形象 AIME Logo | `icon/aime_logo/{filename}` | TBD | 待上传 |

### MUST

- MUST use the CDN root `https://cdn.ainvest.com/` — never reference images from local paths or other hosts in production
- MUST match symbol/code casing exactly — paths are case-sensitive (`AAPL` ≠ `aapl`)
- MUST handle load failure gracefully — show fallback background (`color.bg.weak`) or placeholder when CDN image 404s
- **MUST use the correct market-specific CDN sub-path based on the stock's market:**
  - US stocks: `icon/us/{SYMBOL}.png` — e.g. `https://cdn.ainvest.com/icon/us/AAPL.png`
  - HK stocks: `icon/hk/{SYMBOL}.png` — e.g. `https://cdn.ainvest.com/icon/hk/700.png`
  - A-Share stocks: `icon/a/{SYMBOL}.png` — e.g. `https://cdn.ainvest.com/icon/a/600519.png`
  - MUST NOT use `/us/` path for HK or A-Share tickers — it will always 404

### MUST NOT

- MUST NOT hardcode image filenames for financial data assets — always construct from live data (symbol, code, ID)
- MUST NOT cache financial data asset URLs beyond 30d in local state

---

### 6.3 Politician Avatar Local Assets

Local politician avatar files live at `ainvest-design-system/assets/avatars/politicians/`.

When generating HTML pages served from `temp/`, reference these files with the relative path `../ainvest-design-system/assets/avatars/politicians/{Firstname_Lastname}.png`.

**Available files (as of current session):**

| File | Politician |
|---|---|
| `Nancy_Pelosi.png` | Nancy Pelosi (D · CA) |
| `Marjorie_Taylor_Greene.png` | Marjorie Taylor Greene (R · GA) |
| `Ro_Khanna.png` | Ro Khanna (D · CA) |
| `Sheldon_Whitehouse.png` | Sheldon Whitehouse (D · RI) |
| `Shelley_Moore_Capito.png` | Shelley Moore Capito (R · WV) |
| `Markwayne_Mullin.png` | Markwayne Mullin (R · OK) |
| `Tina_Smith.png` | Tina Smith (D · MN) |
| `Debbie_Wasserman_Schultz.png` | Debbie Wasserman Schultz (D · FL) |
| `Cleo_Fields.png` | Cleo Fields (D · LA) |
| `Delaney_April_McClain.png` | Delaney April McClain |
| `Gil_Cisneros.png` | Gil Cisneros (D · CA) |
| `Jonathan_Jackson.png` | Jonathan Jackson (D · IL) |

**For politicians not in the list above:** render the initials fallback (see §2 MUST).

---

## 7. Functional Icons

### Rule — Local SVG + currentColor, placeholder if missing

UI functional icons (arrows, chevrons, actions, status indicators) MUST be loaded from `assets/icons/{name}.svg` and colored via `currentColor`. Missing icons MUST render a visible placeholder, never empty space.

### MUST

- MUST use SVG from `assets/icons/` — inline or via `<img src="icons/{name}.svg">`
- MUST use `fill="currentColor"` / `stroke="currentColor"` inside the SVG so the icon inherits the parent's CSS `color`
- MUST render a placeholder when the required icon is not yet synced locally: a `1px dashed` border box at the icon's target size, plus an HTML comment `<!-- TODO: sync icon "{name}" from Matrix gallery -->`
- MUST size icons with explicit `width` / `height` (CSS or attributes) to prevent layout shift — typical sizes: `12px`, `16px`, `20px`, `24px`

### MUST NOT

- MUST NOT use icon fonts (Font Awesome, Material Icons, etc.)
- MUST NOT use raster PNG/JPG as functional icons
- MUST NOT hardcode `fill="#xxx"` in icon SVGs — it breaks `currentColor` theming
- MUST NOT silently omit missing icons — always show the placeholder so the gap is visible
- MUST NOT fetch icons from the Matrix gallery URL at runtime — icons are synced by humans into `assets/icons/` (see `assets/icons/README.md`)

### Source

Icons come from the internal Ainvest Matrix library (`@ainvest-matrix/icons`). Gallery: https://f2e.myhexin.com/thor/storybook/ainvest-matrix-react/?path=/story/icons-gallery--gallery. Sync process is manual — see `assets/icons/README.md`.

### Example

```html
<!-- Present locally: inherits color from parent -->
<span style="color: var(--color-text-primary)">
  <img src="icons/arrow-up.svg" width="16" height="16" alt="" />
</span>

<!-- Missing: visible placeholder + TODO -->
<!-- TODO: sync icon "custom-spark" from Matrix gallery -->
<span style="display:inline-block;width:16px;height:16px;border:1px dashed var(--color-border-weak);"></span>
```

---

## FAILURE

- If a ticker logo appears square or has visible corners → add `overflow: hidden` + `border-radius: 50%` to the **container**
- If an avatar appears square → same fix as above
- If an image stretches or distorts → add `object-fit: cover`
- If layout shifts when images load → add explicit `width` and `height` attributes
- If a stock ticker logo renders as a colored blob or solid circle → you used `.svg`; switch to `.png`
- If a politician avatar fails to load → use initials fallback, NOT CDN URL
