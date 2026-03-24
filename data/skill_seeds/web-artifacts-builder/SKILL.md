---
name: web-artifacts-builder
description: "Use when building complex, multi-component claude.ai HTML artifacts that need React, Tailwind CSS, shadcn/ui, state management, or routing. Initializes a full frontend project, develops the artifact, and bundles everything into a single self-contained HTML file. Not for simple single-file HTML/JSX artifacts."
license: Complete terms in LICENSE.txt
---

# Web Artifacts Builder

Build elaborate claude.ai artifacts using React 18 + TypeScript + Vite + Tailwind CSS + shadcn/ui, bundled into a single HTML file.

## Workflow

1. **Initialize** project with `bash scripts/init-artifact.sh <project-name>`
2. **Develop** by editing generated files (React components, styles, logic)
3. **Bundle** into single HTML with `bash scripts/bundle-artifact.sh`
4. **Share** the `bundle.html` as a claude.ai artifact
5. **Test** (optional) - only if requested or issues arise

## Step 1: Initialize

```bash
bash scripts/init-artifact.sh <project-name>
```

Creates a project with:
- React + TypeScript (Vite)
- Tailwind CSS 3.4.1 with shadcn/ui theming
- Path aliases (`@/`) configured
- 40+ shadcn/ui components pre-installed
- All Radix UI dependencies
- Parcel configured for bundling
- Node 18+ compatibility

## Step 2: Develop

Edit the generated files. See shadcn/ui docs for component usage: https://ui.shadcn.com/docs/components

### Design Guidelines

Avoid generic "AI slop" aesthetics:
- No excessive centered layouts
- No purple gradients
- No uniform rounded corners everywhere
- No Inter font as default

## Step 3: Bundle

```bash
bash scripts/bundle-artifact.sh
```

Produces `bundle.html` - a self-contained artifact with all JS, CSS, and dependencies inlined.

**Requires**: `index.html` in project root.

**What it does**:
- Installs bundling deps (parcel, html-inline, parcel-resolver-tspaths)
- Creates `.parcelrc` with path alias support
- Builds with Parcel (no source maps)
- Inlines all assets into single HTML

## Step 4: Share

Share `bundle.html` in the conversation. It renders as an interactive artifact in claude.ai or works in any browser.

## Step 5: Test (Optional)

Use available tools (Playwright, Puppeteer, other skills) to verify if needed. Avoid upfront testing to reduce latency - test after presenting the artifact.

## Validation Checklist

- [ ] `bundle.html` opens in browser without errors
- [ ] All interactive elements function correctly
- [ ] No external dependencies required (everything inlined)
- [ ] Design avoids generic AI aesthetics
- [ ] Responsive layout works at common viewport sizes
