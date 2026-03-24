---
name: microsim-generator
description: "Create interactive educational MicroSims by routing requests to the best-matched JavaScript library (p5.js, Chart.js, Plotly, Mermaid, vis-network, vis-timeline, Leaflet, Venn.js). Use when users request interactive visualizations, data charts, timelines, maps, network diagrams, flowcharts, mathematical plots, set diagrams, priority matrices, comparison tables, or custom simulations and animations."
---

# MicroSim Generator

Meta-skill that routes MicroSim creation requests to the appropriate specialized generator. Consolidates 13 individual generator skills into a single entry point with on-demand loading of implementation guides.

## Step 1: Analyze Request and Match Generator

Scan the user's request for trigger keywords and match to the appropriate generator guide.

### Quick Reference Routing Table

| Trigger Keywords | Guide File | Library |
|------------------|------------|---------|
| timeline, dates, chronological, events, history, schedule, milestones | `references/timeline-guide.md` | vis-timeline |
| map, geographic, coordinates, latitude, longitude, locations, markers | `references/map-guide.md` | Leaflet.js |
| function, f(x), equation, plot, calculus, sine, cosine, polynomial | `references/plotly-guide.md` | Plotly.js |
| network, nodes, edges, graph, dependencies, concept map, knowledge graph | `references/vis-network-guide.md` | vis-network |
| flowchart, workflow, process, state machine, UML, sequence diagram | `references/mermaid-guide.md` | Mermaid.js |
| venn, sets, overlap, intersection, union, categories | `references/venn-guide.md` | Custom |
| chart, bar, line, pie, doughnut, radar, statistics, data | `references/chartjs-guide.md` | Chart.js |
| bubble, priority, matrix, quadrant, impact vs effort, risk vs value | `references/bubble-guide.md` | Chart.js |
| causal, feedback, loop, systems thinking, reinforcing, balancing | `references/causal-loop-guide.md` | vis-network |
| comparison, table, ratings, stars, side-by-side, features | `references/comparison-table-guide.md` | Custom |
| animation, celebration, particles, confetti, effects | `references/celebration-guide.md` | p5.js |
| custom, simulation, physics, interactive, bouncing, movement, p5.js | `references/p5-guide.md` | p5.js |

### Decision Tree

```
Has dates/timeline/chronological events?
  → YES: timeline-guide.md

Has geographic coordinates/locations?
  → YES: map-guide.md

Mathematical function f(x) or equation?
  → YES: plotly-guide.md

Nodes and edges/network relationships?
  → YES: vis-network-guide.md (or causal-loop-guide.md if systems thinking)

Flowchart/workflow/process diagram?
  → YES: mermaid-guide.md

Sets with overlaps (2-4 categories)?
  → YES: venn-guide.md

Priority matrix/2x2 quadrant/multi-dimensional?
  → YES: bubble-guide.md

Standard chart (bar/line/pie/radar)?
  → YES: chartjs-guide.md

Comparison table with ratings/stars?
  → YES: comparison-table-guide.md

Celebration/particles/visual feedback?
  → YES: celebration-guide.md

Custom simulation/animation/physics?
  → YES: p5-guide.md
```

## Step 2: Load the Matched Guide

Once you identify the best generator, **read the corresponding guide file** from the `references/` directory and follow its workflow.

Example:
- User asks for "a timeline showing the history of Unix"
- Match: `timeline` keyword → Load `references/timeline-guide.md`
- Follow the timeline-guide.md workflow

## Step 3: Execute Generator Workflow

Each guide contains:
1. Library-specific requirements
2. Directory structure to create
3. Step-by-step implementation workflow
4. Code templates and patterns
5. Best practices for that visualization type

## Handling Ambiguous Requests

If the request could match multiple generators:

1. **Read `references/routing-criteria.md`** for detailed scoring methodology
2. **Score top 3 candidates** using the 0-100 scale
3. **Present options to user** with reasoning:
   ```
   Based on your request, I recommend:
   1. [Generator A] (Score: 85) - Best for [reason]
   2. [Generator B] (Score: 70) - Alternative if you need [feature]
   3. [Generator C] (Score: 55) - Possible if [condition]

   Which would you prefer?
   ```
4. **Proceed with user's selection**

## Common Ambiguities

| Ambiguous Term | Clarification Needed |
|----------------|---------------------|
| "graph" | Chart (ChartJS) or Network graph (vis-network)? |
| "diagram" | Structural (Mermaid), Network (vis-network), or Custom (p5)? |
| "map" | Geographic (Leaflet) or Concept map (vis-network)? |
| "visualization" | What type of data? What interaction needed? |

## Shared Standards

All MicroSims follow these standards regardless of generator:

**Directory Structure:**
```
docs/sims/<microsim-name>/
├── main.html       # Main visualization file
├── index.md        # Documentation page
├── *.js or *.css   # Supporting files
└── metadata.json   # Dublin Core metadata (optional)
```

**Integration:**
- Embedded via iframe in MkDocs pages
- Width-responsive design
- Non-scrolling iframe container
- Standard height: drawHeight + controlHeight + 2px

**Quality Checklist:**
- [ ] Runs without errors in modern browsers
- [ ] Responsive to container width
- [ ] Controls respond immediately
- [ ] Educational purpose is clear
- [ ] Code is well-commented

## Reference Files

- `references/routing-criteria.md` - Complete scoring methodology for all generators
- `references/<generator>-guide.md` - Specific implementation guide for each generator
- `assets/templates/` - Shared templates and patterns

## mkdocs.yml Integration

After creating a MicroSim, add it to the site navigation:

```yaml
nav:
  - MicroSims:
    - List of MicroSims: sims/index.md
    - New MicroSim: sims/new-microsim-name/index.md
```
