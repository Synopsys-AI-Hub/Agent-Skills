---
name: synopsys-brand
description: Applies Synopsys' official brand colors, typography, and design tokens to any sort of artifact that may benefit from having Synopsys' look-and-feel. Use it when brand colors or style guidelines, visual formatting, or company design standards apply.
license: Complete terms in LICENSE.txt
---

# Synopsys Brand Styling

## Overview

To access Synopsys' official brand identity and style resources, use this skill.

**Keywords**: branding, corporate identity, visual identity, post-processing, styling, brand colors, typography, Synopsys brand, visual formatting, visual design, EDA, semiconductor

## Brand Guidelines

### Colors

**Primary Colors:**

- Purple: `#7E45AF` - Primary brand purple — buttons, CTAs, accents, interactive elements
- Purple Dark: `#5A2A82` - Dark purple — headings, section titles, hover states
- Purple Deep: `#2D1541` - Deepest purple — gradient start, dark hero backgrounds
- Purple Logo: `#573885` - Logo wordmark purple (use only for logo rendering)

**Neutral Colors:**

- Text Primary: `#111C24` - Primary body text, footer background, dark UI surfaces
- Text Secondary: `#2B333F` - Secondary text, subheadings
- Grey: `#555555` - Muted text, captions, metadata
- Grey Medium: `#666666` - Tertiary text, placeholders
- Grey Light: `#B3B4B3` - Borders, dividers, disabled states
- Background Light: `#E3E3E3` - Light backgrounds, cards
- Background Page: `#E6EAED` - Page/canvas background
- Background Section: `#F7F7FA` - Alternating section backgrounds
- Background Surface: `#F4F4F4` - Card/panel surface
- White: `#FFFFFF` - White surfaces, text on dark
- Black: `#000000` - Pure black (use sparingly)
- Dark Surface: `#151B17` - Dark section backgrounds, video overlays
- Dark Surface Alt: `#171917` - Alternative dark surface

**Accent Colors:**

- Link Blue: `#316ACA` - Hyperlinks, inline text links
- Accent Gold: `#FDB71A` - Accent highlights, gradient endpoints, badges
- Accent Warm: `#AB714E` - Warm accent, gradient midpoints
- Slate: `#646E81` - Slate grey — gradient endpoint, muted UI

### Typography

- **Headings**: Roboto (with Arial fallback)
- **Body Text**: Roboto (with Arial fallback)
- **Fallback Stack**: `Roboto, Arial, sans-serif`
- **Note**: Roboto is the sole brand typeface. Do not use serif, monospace, or decorative faces for brand content.

### Gradients

**Purple Brand Gradient** (hero banners, feature sections):
```css
background: linear-gradient(107.7deg, #2D1541 0%, #5A2A82 50%, #7E45AF 100%);
```

**Dark Gradient** (dark sections, overlays):
```css
background: linear-gradient(107.7deg, #000000 0%, #191C20 35%, #646E81 100%);
```

**Light Fade** (page sections):
```css
background: linear-gradient(-180deg, #FFFFFF 88%, #73859F 100%);
```

**Conic Accent Gradient** (decorative borders, badges):
```css
background: conic-gradient(from 90deg, #FDB71A 0deg, #AB714E 25%, #5A2A82 50%, #AB714E 75%, #FDB71A 100%);
```

## Features

### Smart Font Application

- Applies Roboto font to all text elements
- Headings use weight 400–700; body text uses weight 300–400
- Automatically falls back to Arial if Roboto is unavailable
- Preserves readability across all systems

### Text Styling

- Headings: Roboto, `#5A2A82` on light backgrounds, `#FFFFFF` on dark
- Body text: Roboto, `#111C24` on light backgrounds, `#FFFFFF` on dark
- Links: `#316ACA` in all contexts
- Body font size: 15px, line-height 24px (1.6)
- Heading sizes vary; button/CTA text at 16px
- Preserves text hierarchy and formatting

### Button and CTA Styling

- Primary CTA: `background: #7E45AF`, `color: #FFFFFF`, `border-radius: 5px`, `padding: 0 24px`
- Secondary/Outline CTA: `background: transparent`, `color: #111C24`, `border: 1px solid #111C24`, `border-radius: 5px`
- Link CTA: `color: #316ACA`, no background, no border

### Card and Panel Styling

- White surface (`#FFFFFF`) with subtle shadow or light border
- Border radius: `10px`
- Section alternating backgrounds: `#F7F7FA` or `#F4F4F4`

### Footer Styling

- Background: `#111C24`
- Primary text: `#FFFFFF`
- Secondary links: `#B3B4B3`

### Shape and Accent Colors

- Non-text shapes and decorative elements use accent colors
- Cycles through purple, blue, gold, and warm brown accents
- Maintains visual interest while staying on-brand

### Chart and Data Visualization Palette

Categorical series colors in order:
1. `#7E45AF` — Purple (primary)
2. `#316ACA` — Blue
3. `#FDB71A` — Gold
4. `#5A2A82` — Dark purple
5. `#AB714E` — Warm brown
6. `#646E81` — Slate
7. `#111C24` — Dark
8. `#B3B4B3` — Grey

Sequential/single-hue scale: `#2D1541` → `#5A2A82` → `#7E45AF` → `#C9A2E4`

## Technical Details

### Font Management

- Uses system-installed Roboto font when available
- Provides automatic fallback to Arial
- No font installation required — works with existing system fonts
- For best results, pre-install Roboto in your environment

### Color Application

- Uses RGB color values for precise brand matching
- Applied via python-pptx's RGBColor class for presentations
- CSS custom properties for web artifacts
- Maintains color fidelity across different systems

### CSS Custom Properties (Copy-Paste Ready)

```css
:root {
  /* Synopsys Brand Tokens */
  --snps-purple: #7E45AF;
  --snps-purple-dark: #5A2A82;
  --snps-purple-deep: #2D1541;
  --snps-text: #111C24;
  --snps-text-secondary: #2B333F;
  --snps-grey: #555555;
  --snps-grey-light: #B3B4B3;
  --snps-bg-page: #E6EAED;
  --snps-bg-section: #F7F7FA;
  --snps-bg-surface: #F4F4F4;
  --snps-white: #FFFFFF;
  --snps-dark-surface: #151B17;
  --snps-link-blue: #316ACA;
  --snps-accent-gold: #FDB71A;
  --snps-accent-warm: #AB714E;
  --snps-slate: #646E81;
  --snps-radius: 10px;
  --snps-font: Roboto, Arial, sans-serif;
}
```

### Design Rules

1. **Purple is the signature.** Use `#7E45AF` for primary actions and `#5A2A82` for headings — never use both at the same weight in the same context.
2. **Dark backgrounds are near-black, not pure black.** Prefer `#111C24` or `#151B17` over `#000000`.
3. **Gradients flow left-to-right** at approximately 108° angle.
4. **Gold (`#FDB71A`) is a sparing accent** — decorative borders or highlight moments, never large surfaces.
5. **Roboto is the only brand typeface.** Fall back to Arial, then sans-serif.
6. **Maintain high contrast.** White text on purple/dark backgrounds; dark text (`#111C24`) on light backgrounds.
7. **Logo clear space** equals the x-height of the "S" in the Synopsys wordmark. Never skew, rotate, shadow, or modify the logo.
8. **Tagline:** "Our Technology, Your Innovation™"
