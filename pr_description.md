Hey @ynulihao 👋

I ran your skills through `tessl skill review` at work and found some targeted improvements. Here's the full before/after:

| Skill | Before | After | Change |
|-------|--------|-------|--------|
| analyzing-financial-statements | 28% | 83% | +55% |
| creating-financial-models | 36% | 83% | +47% |
| theme-factory | 45% | 89% | +44% |
| treatment-plans | 48% | 89% | +41% |
| fabric | 62% | 100% | +38% |
| employment-contract-templates | 57% | 93% | +36% |
| postgresql-table-design | 56% | 90% | +34% |
| research-grants | 59% | 93% | +34% |
| prompt-engineering-patterns | 62% | 95% | +33% |
| brand-guidelines | 56% | 86% | +30% |
| canvas-design | 47% | 77% | +30% |
| markitdown | 66% | 94% | +28% |
| email-composer | 72% | 100% | +28% |
| data-visualization | 69% | 96% | +27% |
| clinical-decision-support | 51% | 78% | +27% |
| web-artifacts-builder | 70% | 96% | +26% |
| microsim-generator | 76% | 100% | +24% |
| marketing-demand-acquisition | 78% | 100% | +22% |
| data-storytelling | 74% | 91% | +17% |
| algorithmic-art | 68% | 85% | +17% |
| skill-creator | 76% | 93% | +17% |
| webapp-testing | 84% | 100% | +16% |
| code-review-excellence | 72% | 87% | +15% |
| pdf | 79% | 94% | +15% |
| frontend-design | 71% | 85% | +14% |
| openai-api | 83% | 96% | +13% |
| sql-optimization-patterns | 81% | 94% | +13% |
| mcp-builder | 80% | 93% | +13% |
| api-integration-specialist | 79% | 90% | +11% |
| internal-comms | 84% | 93% | +9% |
| statistical-analysis | 84% | 93% | +9% |
| docx | 85% | 93% | +8% |
| diagramming | 83% | 90% | +7% |
| doc-coauthoring | 80% | 87% | +7% |
| social-media-generator | 80% | 86% | +6% |
| beads | 88% | 93% | +5% |
| joke-engineering | 84% | 89% | +5% |
| resolve-conflicts | 80% | 85% | +5% |
| slack-gif-creator | 84% | 89% | +5% |
| pptx | 85% | 89% | +4% |
| auth-implementation-patterns | 83% | 86% | +3% |

**Average score: 75% → 91% (+16%)**

> Note: 7 skills originally scored 0% due to frontmatter validation issues (non-kebab-case names, `allowed-tools` as arrays instead of strings, unquoted YAML values). I fixed those first, then re-scored to get meaningful baselines — the "Before" column above reflects the corrected baseline scores, not the original 0%.

<details>
<summary>Changes summary</summary>

### Frontmatter fixes (7 skills)
- Fixed `name` field to kebab-case: `api-integration-specialist`, `email-composer`
- Fixed `allowed-tools` from YAML array to quoted string: `clinical-decision-support`, `markitdown`, `research-grants`, `treatment-plans`
- Fixed unquoted YAML description causing parse errors: `ffmpeg-color-grading-chromakey`

### Description improvements (40+ skills)
- Added explicit "Use when..." clauses with natural trigger terms
- Increased specificity with concrete actions and capabilities
- Converted chevron (`>`, `|`) descriptions to quoted strings

### Content improvements (40+ skills)
- Removed boilerplate explanations Claude already knows (e.g., "what is authentication", basic HTML structure)
- Added structured workflows with numbered steps and validation checkpoints
- Used progressive disclosure — moved lengthy examples to reference files
- Consolidated redundant sections (merged duplicate "When to Use" lists into descriptions)
- Replaced verbose code blocks with concise patterns
- Used tables for scannable reference material (diagram types, strategy matrices, platform guidelines)

### Significant reductions
- `treatment-plans`: 1,577 → ~130 lines
- `marketing-demand-acquisition`: 985 → ~97 lines
- `research-grants`: 934 → ~127 lines
- `employment-contract-templates`: 508 → ~97 lines
- `clinical-decision-support`: 505 → ~160 lines
- `data-storytelling`: 424 → ~73 lines
- `algorithmic-art`: 405 → ~97 lines

</details>

Honest disclosure — I work at @tesslio where we build tooling around skills like these. Not a pitch — just saw room for improvement and wanted to contribute.

Want to self-improve your skills? Just point your agent (Claude Code, Codex, etc.) at [this Tessl guide](https://docs.tessl.io/evaluate/optimize-a-skill-using-best-practices) and ask it to optimize your skill. Ping me — [@rohan-tessl](https://github.com/rohan-tessl) — if you hit any snags.

Thanks in advance 🙏
