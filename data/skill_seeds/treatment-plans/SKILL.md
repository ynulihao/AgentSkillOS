---
name: treatment-plans
description: "Use when a clinician or user needs to create an individualized patient treatment plan. Generates concise (1-4 page) medical treatment plans in LaTeX/PDF format for all clinical specialties including general medicine, rehabilitation, mental health, chronic disease, perioperative care, and pain management. Features SMART goal frameworks, evidence-based interventions, HIPAA compliance, and professional formatting with colored information boxes."
allowed-tools: "Read, Write, Edit, Bash"
---

# Treatment Plan Writing

Generate concise, clinically actionable treatment plans across all medical specialties with professional LaTeX/PDF output.

## Workflow

1. **Assess clinical context** — Identify specialty, condition complexity, and appropriate format (1-page, 3-4 page, or 5-6 page)
2. **Select template** — Choose from `one_page_treatment_plan.tex` (preferred), or specialty templates in `assets/`
3. **Build first-page executive summary** — Create full-page summary with colored tcolorbox elements (goals, interventions, decision points)
4. **Draft detailed sections** — Add SMART goals, interventions, monitoring, and follow-up (pages 2+)
5. **Generate scientific schematics** — Run `python scripts/generate_schematic.py "description" -o figures/output.png` for at least 1 figure
6. **Validate** — Run `python check_completeness.py plan.tex` and `python validate_treatment_plan.py plan.tex`
7. **Compile** — Run `xelatex plan.tex` to produce final PDF

## Document Length Guidelines

| Complexity | Format | When to Use |
|---|---|---|
| Simple/standard | 1 page (PREFERRED) | Straightforward protocols, busy clinical settings |
| Moderate | 3-4 pages | Multidisciplinary coordination, patient education needed |
| High (rare) | 5-6 pages | Multiple comorbidities, research protocols |

## First-Page Executive Summary (MANDATORY)

Every treatment plan must begin with a full-page executive summary before any TOC or detailed sections:

1. **Document title and subtitle** — Plan type + specific condition
2. **Report information box** (`\begin{patientinfo}`) — Type, date, demographics, diagnosis (ICD-10), framework
3. **Key findings boxes** (2-4 colored `tcolorbox`):
   - `\begin{goalbox}` — Primary SMART goals
   - `\begin{keybox}` — Core interventions
   - `\begin{warningbox}` — Critical decision points (if applicable)
   - `\begin{infobox}` — Timeline overview

Use `\thispagestyle{empty}` and end with `\newpage`. TOC on page 2, detailed content page 3+.

## Clinical Specialties Supported

### General Medical Treatment Plans
Standard components: de-identified patient info, diagnosis with ICD-10, SMART goals (short-term 1-3 months, long-term 6-12 months), pharmacological/non-pharmacological/procedural interventions, timeline, monitoring parameters, expected outcomes, follow-up plan, patient education, risk mitigation.

Common conditions: diabetes, hypertension, heart failure, COPD, asthma, hyperlipidemia, CKD.

### Rehabilitation Plans
Functional assessment (FIM, Barthel, Berg Balance), impairment/activity/participation-level goals, PT/OT/SLP interventions, treatment schedule, home exercise program.

Post-stroke, orthopedic, cardiac, pulmonary, vestibular, neurological, sports injury rehabilitation.

### Mental Health Plans
Psychiatric assessment (DSM-5), symptom severity scales (PHQ-9, GAD-7), psychotherapy modalities (CBT, DBT, ACT, IPT), psychopharmacology with titration, safety planning with crisis contacts, monitoring.

MDD, anxiety disorders, bipolar, schizophrenia, PTSD, eating disorders, substance use, personality disorders.

### Chronic Disease Management
Disease-specific targets per guidelines, self-management support, care coordination across providers, population health integration. Diabetes, CVD, COPD, CKD, IBD, autoimmune conditions, HIV, cancer survivorship.

### Perioperative Care Plans
Preoperative risk stratification (ASA class), ERAS protocols, VTE/antibiotic prophylaxis, postoperative recovery goals, discharge planning.

### Pain Management Plans
Multimodal approach: non-opioid analgesics, adjuvant medications, interventional procedures (nerve blocks, SCS), non-pharmacological (PT, CBT, mindfulness). Opioid safety protocols when prescribed (PDMP check, naloxone, treatment agreements).

## Scientific Schematics (MANDATORY)

Every treatment plan must include at least 1 AI-generated figure:

```bash
python scripts/generate_schematic.py "treatment pathway flowchart for [condition]" -o figures/output.png
```

Suitable figures: treatment pathway flowcharts, care coordination diagrams, therapy timelines, decision algorithms, medication management flowcharts.

## SMART Goal Framework

All goals must be:
- **Specific**: "Reduce HbA1c to <7%" not "Better diabetes control"
- **Measurable**: Quantifiable metrics or validated scales
- **Achievable**: Realistic given patient capabilities and resources
- **Relevant**: Aligned with patient values and priorities
- **Time-bound**: Clear timeframe (e.g., "within 8 weeks")

## Citations

Use minimal, targeted citations (0-3 per plan):
- Cite: guideline recommendations, specific dosing protocols, novel interventions
- Skip: standard-of-care, basic medical facts, routine practices
- Format: inline (e.g., "per ADA Standards of Care 2024")

## Professional Styling

Use `medical_treatment_plan.sty` from `assets/` for professional formatting:

| Box Type | Environment | Use For |
|---|---|---|
| Info (blue) | `\begin{infobox}` | Assessments, schedules, protocols |
| Warning (red/yellow) | `\begin{warningbox}` | Safety alerts, decision points |
| Goal (green) | `\begin{goalbox}` | Treatment targets, SMART goals |
| Key Points (blue bg) | `\begin{keybox}` | Executive summary, key recommendations |
| Emergency (red) | `\begin{emergencybox}` | Emergency contacts, urgent protocols |
| Patient Info (white) | `\begin{patientinfo}` | Demographics, baseline info |

Compile with XeLaTeX for best results: `xelatex plan.tex`

## Templates

Located in `assets/`:
- `one_page_treatment_plan.tex` — FIRST CHOICE for most cases
- `general_medical_treatment_plan.tex` — Primary care, chronic disease
- `rehabilitation_treatment_plan.tex` — PT/OT, post-surgery
- `mental_health_treatment_plan.tex` — Psychiatric, behavioral health
- `chronic_disease_management_plan.tex` — Complex chronic diseases
- `perioperative_care_plan.tex` — Surgical patients
- `pain_management_plan.tex` — Acute/chronic pain

## Validation Scripts

- `check_completeness.py` — Verifies all required sections present
- `validate_treatment_plan.py` — SMART goal assessment, evidence verification, safety review
- `timeline_generator.py` — Generates visual treatment timelines
- `generate_template.py` — Interactive template selection

## Integration

- **clinical-reports**: SOAP notes, H&P, discharge summaries
- **scientific-writing**: Citation management, evidence synthesis
- **scientific-schematics**: TikZ flowcharts for decision algorithms
- **clinical-decision-support**: Population-level analyses (this skill handles individual patients)

## Key Principles

1. **Brevity first** — Every sentence must add clinical value; think "quick reference card" not "textbook"
2. **First page is king** — A strong executive summary can often stand alone
3. **Patient-centered** — Shared decision-making, cultural competence, patient preferences
4. **HIPAA compliant** — De-identify all 18 identifiers per Safe Harbor method
5. **Evidence-based** — Follow current specialty society guidelines
