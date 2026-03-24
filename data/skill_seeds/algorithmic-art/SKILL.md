---
name: algorithmic-art
description: "Use when creating generative or algorithmic art with p5.js. Produces interactive HTML artifacts with seeded randomness and parameter controls for flow fields, particle systems, noise-based compositions, and other computational aesthetics. Creates original algorithmic art to avoid copyright issues."
license: Complete terms in LICENSE.txt
---

Create algorithmic art in two phases: (1) write an algorithmic philosophy as `.md`, then (2) express it as an interactive p5.js artifact (`.html`).

## Workflow

1. **Interpret** the user's aesthetic intent
2. **Create algorithmic philosophy** (4-6 paragraphs, saved as `.md`)
3. **Read** `templates/viewer.html` as the literal starting point
4. **Implement** p5.js algorithm expressing the philosophy
5. **Design parameters** and matching UI controls
6. **Validate** seed reproducibility and interactive controls

## Phase 1: Algorithmic Philosophy

Create a computational aesthetic manifesto (not static images):

- **Name the movement** (1-2 words): e.g., "Organic Turbulence", "Quantum Harmonics"
- **Articulate the philosophy** (4-6 paragraphs) covering:
  - Computational processes and mathematical relationships
  - Noise functions, particle behaviors, field dynamics
  - Temporal evolution and parametric variation
  - How emergent complexity arises from simple rules
- **Emphasize craftsmanship**: The algorithm must feel meticulously refined by a master of computational aesthetics
- **Leave creative space**: Be specific about direction but concise enough for interpretive implementation choices

Output as a `.md` file.

### Philosophy Examples (condensed)

- **"Organic Turbulence"**: Flow fields from layered Perlin noise. Thousands of particles following vector forces, trails accumulating into density maps. Color from velocity and density.
- **"Quantum Harmonics"**: Grid particles with evolving phase values. Interference patterns create bright nodes and voids. Simple harmonic motion yields complex emergent mandalas.
- **"Recursive Whispers"**: Branching L-systems with golden ratio constraints. Noise perturbations break symmetry. Line weights diminish per recursion level.
- **"Field Dynamics"**: Vector fields from mathematical functions. Particles flow along field lines, leaving ghost-like traces of invisible forces.

## Phase 2: Deduce Conceptual Seed

Before coding, identify the subtle conceptual thread from the original request. The reference should be refined enough that knowledgeable viewers feel it intuitively while others simply appreciate the generative beauty.

## Phase 3: p5.js Implementation

### Critical: Start from Template

1. **Read** `templates/viewer.html` with the Read tool
2. **Keep fixed**: header, sidebar structure, Anthropic branding, seed controls, action buttons
3. **Replace only**: p5.js algorithm, parameters object, parameter UI controls

### Technical Requirements

```javascript
// Always use seeded randomness
let seed = 12345;
randomSeed(seed);
noiseSeed(seed);

let params = {
  seed: 12345,
  // Define parameters natural to YOUR algorithm:
  // quantities, scales, probabilities, ratios, angles, thresholds
};
```

Let the philosophy dictate the algorithm:
- **Organic emergence** -> elements that grow, random processes constrained by natural rules, feedback loops
- **Mathematical beauty** -> geometric ratios, trigonometric harmonics, precise calculations
- **Controlled chaos** -> random variation within boundaries, bifurcation, order from disorder

### Craftsmanship Standards

- **Balance**: Complexity without noise, order without rigidity
- **Color harmony**: Thoughtful palettes, not random RGB
- **Composition**: Visual hierarchy even in randomness
- **Performance**: Smooth real-time execution
- **Reproducibility**: Same seed always produces identical output

### Output Format

Single self-contained HTML file built from `templates/viewer.html`:
- p5.js loaded from CDN (`https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.7.0/p5.min.js`)
- All code inline (no external files except CDN)
- Works in claude.ai artifacts or any browser

### Sidebar Structure

1. **Seed (fixed)**: Display, Prev/Next/Random/Jump controls
2. **Parameters (variable)**: Sliders/inputs for algorithm-specific params
3. **Colors (optional)**: Color pickers if the art needs adjustable palette
4. **Actions (fixed)**: Regenerate, Reset, Download PNG

## Resources

- `templates/viewer.html` - **Required starting point** for HTML artifacts
- `templates/generator_template.js` - p5.js best practices and code structure reference
