---
name: clinical-decision-support
description: "Use when a pharmaceutical researcher, clinical guideline committee, or medical affairs team needs population-level clinical analyses. Generates patient cohort analyses with biomarker stratification and statistical outcomes, or treatment recommendation reports with GRADE evidence grading and decision algorithms. Outputs publication-ready LaTeX/PDF with survival curves, forest plots, waterfall plots, and TikZ flowcharts for drug development, regulatory submissions, and clinical guideline development."
allowed-tools: "Read, Write, Edit, Bash"
---

# Clinical Decision Support Documents

Generate professional clinical decision support (CDS) documents for pharmaceutical and clinical research settings.

**Two document types:**
1. **Patient Cohort Analysis** — Biomarker-stratified group analyses with statistical outcome comparisons
2. **Treatment Recommendation Reports** — Evidence-based clinical guidelines with GRADE grading and decision algorithms

**Note:** For individual patient treatment plans, use `treatment-plans` instead. This skill focuses on population-level analyses and evidence synthesis.

## Workflow

1. **Define document type** — Cohort analysis or treatment recommendation report
2. **Gather inputs** — Patient data, biomarker profiles, trial results, or evidence sources
3. **Generate scientific schematics** — Run `python scripts/generate_schematic.py "description" -o figures/output.png` for at least 1-2 figures
4. **Build first-page executive summary** — Full-page summary with 3-5 colored tcolorbox elements
5. **Draft detailed sections** — Methods, results, statistical analysis, recommendations (pages 3+)
6. **Run validation** — Execute `python scripts/validate_cds_document.py` for quality and compliance checks
7. **Compile** — `pdflatex document.tex` to produce publication-ready PDF

## First-Page Executive Summary (MANDATORY)

Page 1 must contain ONLY the executive summary with colored tcolorbox elements:

**For Cohort Analyses:**
- Report Information (blue) — Document type, date, disease state, population
- Primary Results (blue) — Main efficacy/outcome findings (ORR, PFS, OS)
- Biomarker Insights (green) — Key molecular subtype findings
- Clinical Implications (orange) — Actionable treatment implications
- Statistical Summary (gray) — Hazard ratios, p-values, key statistics

**For Treatment Recommendations:**
- Report Information — Disease state, guideline version, target population
- Key Recommendations (green) — Top 3-5 GRADE-graded recommendations
- Biomarker Decision Criteria (blue) — Molecular markers influencing selection
- Evidence Summary (gray) — Major supporting trials
- Critical Monitoring (orange/red) — Essential safety monitoring

Use `\thispagestyle{empty}`, end with `\newpage`. TOC on page 2, detailed content page 3+.

## Patient Cohort Analysis Capabilities

- Biomarker-based patient stratification (molecular subtypes, gene expression, IHC)
- Outcome metrics: OS, PFS, ORR, DOR, DCR with statistical analysis
- Survival analysis: Kaplan-Meier curves, log-rank tests, Cox regression
- Subgroup comparisons: hazard ratios, p-values, 95% CI, forest plots
- Visualizations: waterfall plots, swimmer plots, survival curves

**Detailed sections (page 3+):** Cohort characteristics, biomarker stratification, treatment exposure, outcome analysis, statistical methods, subgroup comparisons, safety profile, figures and tables.

## Treatment Recommendation Report Capabilities

- Evidence-based guidelines with GRADE system (1A, 1B, 2A, 2B, 2C)
- Quality of evidence assessment (high, moderate, low, very low)
- Treatment algorithm flowcharts with TikZ diagrams
- Line-of-therapy sequencing based on biomarkers
- Decision pathways with clinical and molecular criteria

**Detailed sections (page 3+):** Clinical context, target population, evidence review, treatment options, GRADE assessment, recommendations by line, biomarker-guided selection, treatment algorithms, monitoring protocol, special populations, references.

## Clinical Features

- **Biomarker Integration**: Genomic alterations, gene expression signatures, IHC markers, PD-L1 scoring
- **Statistical Methods**: Hazard ratios, p-values, CI, survival curves, Cox regression, log-rank tests
- **Evidence Grading**: GRADE system, Oxford CEBM levels
- **Terminology**: SNOMED-CT, LOINC, proper medical and trial nomenclature
- **Compliance**: HIPAA de-identification, confidentiality headers, ICH-GCP alignment

## GRADE Evidence System

| Grade | Meaning |
|---|---|
| 1A | Strong recommendation, high-quality evidence |
| 1B | Strong recommendation, moderate-quality evidence |
| 2A | Weak recommendation, high-quality evidence |
| 2B | Weak recommendation, moderate-quality evidence |
| 2C | Weak recommendation, low-quality evidence |

**Strength**: Strong (benefits clearly outweigh risks), Conditional (trade-offs exist), Research (insufficient evidence)

## Pharmaceutical Use Cases

- **Drug Development**: Phase 2/3 trial analyses, subgroup analyses, companion diagnostic development, regulatory submissions
- **Medical Affairs**: KOL education, medical strategy, advisory board materials, publication planning
- **Clinical Guidelines**: Guideline development with GRADE, consensus recommendations, practice standards
- **Real-World Evidence**: RWE cohort studies, comparative effectiveness, outcomes research, health economics

## Scientific Schematics (MANDATORY)

Every CDS document must include at least 1-2 AI-generated figures:

```bash
python scripts/generate_schematic.py "clinical decision algorithm for [condition]" -o figures/output.png
```

Suitable figures: clinical decision algorithm flowcharts, treatment pathway diagrams, biomarker stratification trees, CONSORT-style patient flow diagrams, survival curve visualizations.

## Best Practices

### Cohort Analyses
1. Document inclusion/exclusion criteria and patient flow transparently
2. Specify assay methods, platforms, cut-points, and validation status for biomarkers
3. Report hazard ratios with 95% CI (not just p-values), include median follow-up time
4. Use standard criteria: RECIST 1.1 for response, CTCAE v5.0 for AEs, ECOG for performance
5. Present median OS/PFS with 95% CI, landmark survival rates, number at risk tables
6. Clearly label exploratory vs pre-planned subgroup analyses

### Treatment Recommendations
1. Use GRADE consistently with documented rationale for each grade
2. Include phase 3 RCTs as primary evidence, supplemented by phase 2 and RWE
3. Link specific biomarkers to therapy recommendations with validated assay methods
4. Make TikZ flowcharts unambiguous with clear yes/no decision points
5. Address special populations (elderly, renal/hepatic impairment)

### General
1. First-page executive summary is MANDATORY — scannable in 60 seconds
2. De-identify per HIPAA Safe Harbor (all 18 identifiers)
3. Use 0.5in margins, professional fonts, color-coded sections
4. Document all statistical methods for reproducibility

## Templates

Located in `assets/`:
- `cohort_analysis_template.tex` — Biomarker-stratified patient cohort analysis
- `treatment_recommendation_template.tex` — Evidence-based guidelines with GRADE
- `clinical_pathway_template.tex` — TikZ decision algorithm flowcharts
- `biomarker_report_template.tex` — Molecular subtype classification
- `evidence_synthesis_template.tex` — Systematic review and meta-analysis

## Scripts

Located in `scripts/`:
- `generate_survival_analysis.py` — Kaplan-Meier curves, log-rank tests, hazard ratios
- `create_waterfall_plot.py` — Best response visualization
- `create_forest_plot.py` — Subgroup analysis with CI
- `create_cohort_tables.py` — Demographics, biomarker frequency, outcomes tables
- `build_decision_tree.py` — TikZ flowchart generation
- `biomarker_classifier.py` — Patient stratification by molecular subtype
- `calculate_statistics.py` — Hazard ratios, Cox regression, Fisher's exact
- `validate_cds_document.py` — Quality and compliance checks
- `grade_evidence.py` — Automated GRADE assessment helper

## Integration

- **scientific-writing**: Citation management, statistical reporting, evidence synthesis
- **clinical-reports**: Medical terminology, HIPAA compliance, regulatory documentation
- **scientific-schematics**: TikZ flowcharts for decision algorithms and treatment pathways
- **treatment-plans**: Individual patient applications of cohort-derived insights (bidirectional)
