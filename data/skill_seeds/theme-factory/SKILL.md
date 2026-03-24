---
name: theme-factory
description: "Use when the user wants to apply consistent visual styling to artifacts such as slides, docs, reports, or HTML landing pages. Provides 10 pre-set professional themes with curated color palettes and font pairings, or generates custom themes on-the-fly. Each theme includes hex codes and complementary font selections."
license: Complete terms in LICENSE.txt
---

# Theme Factory

Apply professional font and color themes to any artifact — slide decks, documents, reports, landing pages, and more.

## Workflow

1. **Show theme showcase** — Display `theme-showcase.pdf` for the user to browse all 10 themes visually. Do not modify this file.
2. **Ask for selection** — Prompt the user to choose a theme or describe a custom one
3. **Wait for confirmation** — Get explicit confirmation of the chosen theme before proceeding
4. **Apply theme** — Read the corresponding theme file from `themes/` and apply colors and fonts consistently throughout the artifact
5. **Verify** — Ensure proper contrast, readability, and consistent visual identity across all pages/sections

## Available Themes

Each theme is defined in the `themes/` directory with complete hex codes and font pairings:

1. **Ocean Depths** — Professional and calming maritime theme
2. **Sunset Boulevard** — Warm and vibrant sunset colors
3. **Forest Canopy** — Natural and grounded earth tones
4. **Modern Minimalist** — Clean and contemporary grayscale
5. **Golden Hour** — Rich and warm autumnal palette
6. **Arctic Frost** — Cool and crisp winter-inspired theme
7. **Desert Rose** — Soft and sophisticated dusty tones
8. **Tech Innovation** — Bold and modern tech aesthetic
9. **Botanical Garden** — Fresh and organic garden colors
10. **Midnight Galaxy** — Dramatic and cosmic deep tones

## Custom Theme Creation

When none of the existing themes fit:

1. Gather the user's description or preferences (mood, brand colors, context)
2. Generate a new theme with appropriate color palette and font pairings
3. Name the theme descriptively (reflecting its visual identity)
4. Show the generated theme for review and approval
5. Apply the approved custom theme following the same process as pre-set themes
