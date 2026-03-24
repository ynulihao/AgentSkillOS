---
name: brand-guidelines
description: "Use when applying Anthropic brand colors, typography, or visual styling to presentations, documents, or artifacts. Applies official color palette, Poppins/Lora fonts, and accent colors to maintain brand consistency."
license: Complete terms in LICENSE.txt
---

# Anthropic Brand Styling

Apply Anthropic's official brand identity to artifacts including presentations, documents, and visual assets.

## Color Palette

**Main Colors:**

| Color | Hex | Usage |
|-------|-----|-------|
| Dark | `#141413` | Primary text, dark backgrounds |
| Light | `#faf9f5` | Light backgrounds, text on dark |
| Mid Gray | `#b0aea5` | Secondary elements |
| Light Gray | `#e8e6dc` | Subtle backgrounds |

**Accent Colors:**

| Color | Hex | Usage |
|-------|-----|-------|
| Orange | `#d97757` | Primary accent |
| Blue | `#6a9bcc` | Secondary accent |
| Green | `#788c5d` | Tertiary accent |

## Typography

| Element | Font | Fallback | Size |
|---------|------|----------|------|
| Headings | Poppins | Arial | 24pt+ |
| Body text | Lora | Georgia | Standard |

## Workflow

1. **Identify target artifact** -- determine format (PPTX, HTML, document)
2. **Apply typography** -- set Poppins for headings (24pt+), Lora for body text
3. **Apply color palette** -- use main colors for text/backgrounds, accents for shapes
4. **Validate output** -- confirm fonts render correctly, colors match hex values, accent colors cycle through orange/blue/green

## Application Rules

- Headings (24pt+) use Poppins; body text uses Lora
- Smart color selection adapts text color based on background luminance
- Non-text shapes cycle through accent colors (orange, blue, green)
- Fallback fonts activate automatically if Poppins/Lora unavailable
- RGB values applied via python-pptx RGBColor class for presentations
