---
name: social-media-generator
description: "Use when the user needs to create social media posts for Twitter, Instagram, LinkedIn, or Facebook. Generates platform-optimized content with proper character limits, hashtag strategies, and tone, then saves posts in an organized folder structure with meaningful filenames."
---

# Social Media Generator

Generate platform-optimized social media content for Twitter, Instagram, LinkedIn, and Facebook, saved in an organized directory structure.

## Workflow

### Step 1: Gather Information

Collect from the user (ask if not provided):
- Event/content name and date/time (DD-MM-YYYY-HHMM format)
- Main message or announcement
- Target audience and brand voice/tone
- Key details, call-to-action, specific hashtags or mentions

### Step 2: Generate Platform-Specific Content

Create content using templates in `assets/`:

| Platform | Template | Char Limit | Hashtags | Tone |
|----------|----------|-----------|----------|------|
| Twitter | `assets/twitter_template.md` | 280 | 1-3 focused | Casual, punchy, timely |
| Instagram | `assets/instagram_template.md` | 2,200 (concise preferred) | 5-15 relevant | Visual-first, engaging |
| LinkedIn | `assets/linkedin_template.md` | 3,000 | 3-5 professional | Professional, value-driven |
| Facebook | `assets/facebook_template.md` | Under 250 for best engagement | 2-3 | Friendly, conversational |

**Platform-specific guidelines:**
- **Twitter:** Concise, clear CTA, emojis for engagement
- **Instagram:** Hook in first line, line breaks for readability, encourage engagement
- **LinkedIn:** Industry insights, bullet points, discussion-prompting questions
- **Facebook:** Include event details, encourage comments and shares

### Step 3: Create File Structure and Write

```
social-media/
├── twitter/event-name-DD-MM-YYYY-HHMM.md
├── instagram/event-name-DD-MM-YYYY-HHMM.md
├── linkedin/event-name-DD-MM-YYYY-HHMM.md
└── facebook/event-name-DD-MM-YYYY-HHMM.md
```

For each platform:
1. Create subdirectory if it doesn't exist
2. Generate content from template
3. Write to file with lowercase-hyphenated name (e.g., `product-launch-15-03-2025-1400.md`)
4. Include metadata at bottom (platform, date, character count)

### Step 4: Review and Confirm

1. Provide summary of created files with key points per platform
2. Flag any character count warnings
3. Offer revisions

## Call-to-Action Best Practices

- Clear and specific with action verbs
- Create urgency when appropriate
- Match platform conventions
