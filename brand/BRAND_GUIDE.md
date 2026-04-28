# 🐻 Kuma Code — Brand Guide

> Visual identity rules. Use these consistently across all touchpoints.

---

## 🎨 Logo

The logo is a **stylized cyber-bear face** representing Kuma Code's identity as a multi-agent AI coding tool.

### Logo files

| File | Use | Size |
|---|---|---|
| `logo-master.svg` | Source vector — never modify | Any |
| `logo-1024.png` | Marketing hero, large displays | 1024×1024 |
| `logo-512.png` | Marketplace listings, social profile | 512×512 |
| `logo-256.png` | GitHub avatar, README header | 256×256 |
| `logo-128.png` | App icon, medium displays | 128×128 |
| `logo-64.png` | Toolbar icons, medium UI | 64×64 |
| `logo-48.png` | Sidebar icons, small UI | 48×48 |
| `logo-32.png` | **Simplified** — Browser favicon, file icon | 32×32 |
| `logo-16.png` | **Simplified** — Tab favicon, tiny UI | 16×16 |
| `logo-monochrome.svg` | VS Code activity bar (theme-aware) | Any |
| `logo-wordmark.svg` | Landing pages, README banners | Wide |
| `favicon.ico` | Browser favicon (multi-size embedded) | 16-256 |

### Two versions

- **Full version** (`logo-full.svg`, used for ≥48px) — has circuit lines, glowing cheeks, layered eye details
- **Simplified mark** (`logo-mark.svg`, used for 16-32px) — solid shapes only, no decorative effects

**Why two:** the full version's effects don't render at tiny sizes — they become noise. The simplified mark stays readable at 16×16.

---

## 🎨 Color Palette

### Primary

| Color | Hex | Use |
|---|---|---|
| **Cyber Black** | `#0A0A0A` | Bear body, primary backgrounds |
| **Cyan Glow** | `#00E5FF` | Accent, highlights, CTAs |
| **Pure White** | `#FFFFFF` | Eye highlights, text on dark |

### Secondary

| Color | Hex | Use |
|---|---|---|
| **Soft Black** | `#1A1A1A` | Inner face, secondary surfaces |
| **Mid Cyan** | `#26C6DA` | Hover states |
| **Dark Cyan** | `#0097A7` | Pressed/active states |

### Backgrounds

| Color | Hex | Use |
|---|---|---|
| **Background Dark** | `#0F0F0F` | Primary dark mode bg |
| **Surface Dark** | `#1A1A1A` | Card, panel backgrounds |
| **Background Light** | `#FAFAFA` | Light mode primary |
| **Surface Light** | `#FFFFFF` | Light mode cards |

### Semantic

| Color | Hex | Use |
|---|---|---|
| **Success** | `#00E676` | Success states |
| **Warning** | `#FFC107` | Warnings |
| **Error** | `#FF5252` | Errors |
| **Info** | `#00E5FF` | Info (matches accent) |

---

## 🔤 Typography

### Primary font: **JetBrains Mono** (or fallback monospace)

For: code displays, logo wordmark, dev-focused UI

```css
font-family: 'JetBrains Mono', 'Fira Code', 'Source Code Pro', 
             ui-monospace, monospace;
```

### Body font: **Inter** (or system-ui)

For: marketing copy, body text, UI labels

```css
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 
             'Segoe UI', sans-serif;
```

### Wordmark style

The "Kuma Code" wordmark uses:
- "KUMA" in **bold black** (matches bear silhouette weight)
- "CODE" in **regular cyan** (lighter, accent color)
- Both in JetBrains Mono, uppercase, tight letter-spacing (-2px)

---

## 📐 Spacing & Sizing

### Logo padding

Always include padding around the logo equal to **at least 10% of its width**.

```
┌────────────────────┐  ← 10% padding (minimum)
│                    │
│   ▣▣ KUMA LOGO ▣▣  │  ← logo
│                    │
└────────────────────┘  ← 10% padding (minimum)
```

### Minimum sizes

- Square logo: never smaller than **16×16**
- Wordmark: never smaller than **120px wide**

If you need smaller — use the favicon or text-only.

---

## ✅ Do's

- ✅ Use on dark backgrounds (logo is designed dark-first)
- ✅ Maintain 10%+ padding
- ✅ Use SVG when possible (scales perfectly)
- ✅ Use simplified mark at 16-32px sizes
- ✅ Keep the cyan accent — it's the brand signature
- ✅ Use monochrome version for VS Code activity bar (theme-aware)

## ❌ Don'ts

- ❌ Don't change the colors (cyan #00E5FF is signature)
- ❌ Don't stretch / distort
- ❌ Don't add drop shadows or filters
- ❌ Don't rotate
- ❌ Don't outline / add borders
- ❌ Don't place on busy backgrounds without solid bg behind logo
- ❌ Don't use full version below 48px (use simplified mark)
- ❌ Don't add other characters/elements next to bear

---

## 🌗 Light vs Dark Mode

The logo is **dark-first** — designed primarily for dark backgrounds (VS Code default theme).

### Dark mode (primary)
Use logo as-is. The cyan glow reads strongly against dark.

### Light mode
For light backgrounds, you have 2 options:
1. **Place logo in a dark container** — recommended (preserves brand)
2. **Use inverted cheek/eye glow** — the cyan still works on white

Avoid placing the logo directly on white without a contrast boost (cyan is bright but loses presence).

---

## 📱 Application examples

| Context | Use | Notes |
|---|---|---|
| VS Code activity bar | `logo-monochrome.svg` | 24px, theme-aware |
| GitHub repo avatar | `logo-256.png` | 256×256 |
| Marketplace listing | `logo-512.png` | 512×512 hero |
| Browser tab | `favicon.ico` | Auto-picks 16/32 |
| README header | `logo-wordmark-1200.png` | Full wordmark |
| Twitter/X profile | `logo-512.png` | Cropped to circle |
| Discord server icon | `logo-512.png` | Auto-cropped |
| Email signature | `logo-128.png` | Inline image |
| Loading spinner | Custom — animate cheek dots | Reuse cyan accent |

---

## 🎨 Code snippets

### Use logo in HTML

```html
<!-- Standalone logo -->
<img src="/logo-512.png" alt="Kuma Code" width="64" height="64" />

<!-- Inline SVG (recommended — sharper, themeable) -->
<object data="/logo-master.svg" type="image/svg+xml" 
        width="64" height="64" aria-label="Kuma Code">
</object>

<!-- Wordmark -->
<img src="/logo-wordmark.svg" alt="Kuma Code" height="40" />
```

### Use logo in React

```tsx
import logo from './assets/logo-512.png';

<img src={logo} alt="Kuma Code" className="w-16 h-16" />
```

### CSS variables (drop into your stylesheet)

```css
:root {
  --kuma-black: #0A0A0A;
  --kuma-soft: #1A1A1A;
  --kuma-cyan: #00E5FF;
  --kuma-cyan-mid: #26C6DA;
  --kuma-cyan-dark: #0097A7;
  --kuma-white: #FFFFFF;
  --kuma-bg-dark: #0F0F0F;
  --kuma-surface-dark: #1A1A1A;
}
```

---

## 📝 Version history

| Version | Date | Notes |
|---|---|---|
| 1.0 | 2026-04-28 | Initial release — V6 Circuit Kuma direction |

---

## 🔗 Files in this brand kit

```
brand/
├── BRAND_GUIDE.md           ← this file
├── logo-master.svg          ← editable source
├── logo-full.svg            ← full version (≥48px)
├── logo-mark.svg            ← simplified version (16-32px)
├── logo-monochrome.svg      ← single-color (VS Code)
├── logo-wordmark.svg        ← logo + text
├── favicon.ico              ← multi-size browser icon
└── png/
    ├── logo-1024.png
    ├── logo-512.png
    ├── logo-256.png
    ├── logo-128.png
    ├── logo-64.png
    ├── logo-48.png
    ├── logo-32.png             ← uses simplified mark
    ├── logo-16.png             ← uses simplified mark
    ├── logo-wordmark-1200.png
    └── logo-wordmark-600.png
```

---

🐻 **Questions?** This is your brand. Iterate freely — but if you change the colors or proportions, update this guide so the team stays consistent.
