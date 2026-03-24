---
name: research-grants
description: "Use when writing research proposals for NSF, NIH, DOE, or DARPA. Covers agency-specific formatting, review criteria, budget preparation, broader impacts, specific aims, innovation narratives, and compliance with submission requirements."
allowed-tools: "Read, Write, Edit, Bash"
---

# Research Grant Writing

Grants are persuasive documents demonstrating scientific rigor, innovation, feasibility, and broader impact. Each agency has distinct priorities, review criteria, and formatting requirements.

## Workflow

1. **Select agency and mechanism** -- identify the funding opportunity and confirm eligibility
2. **Draft specific aims/objectives** -- write the 1-page aims page first (NIH) or objectives outline
3. **Write project narrative** -- develop the research strategy following agency structure
4. **Add figures** -- generate schematics using `python scripts/generate_schematic.py "description" -o figures/output.png`
5. **Prepare budget** -- align line items with proposed activities using agency-specific rules
6. **Internal review** -- circulate to co-investigators, run mock review
7. **Validate compliance** -- check page limits, formatting, required sections with `scripts/compliance_checker.py`
8. **Submit 48 hours early** -- portals crash; never wait until deadline

## Agency Quick Reference

| Agency | Page Limit | Structure | Key Criteria |
|--------|-----------|-----------|--------------|
| **NSF** | 15 pages | Project Description | Intellectual Merit + Broader Impacts (equal weight) |
| **NIH** | 1 + 12 pages (R01) | Specific Aims + Research Strategy | Significance, Innovation, Approach, Investigator, Environment |
| **DOE** | Varies | Project Narrative | Technical merit, approach, personnel, budget, mission relevance |
| **DARPA** | Varies | Technical Volume | "What if true? Who cares?" High-risk/high-reward, transition path |

For detailed agency guidance, load:
- `references/nsf_guidelines.md`, `references/nih_guidelines.md`
- `references/doe_guidelines.md`, `references/darpa_guidelines.md`

## Core Proposal Components

### Executive Summary / Project Summary
- Open with compelling hook establishing importance
- State specific, measurable objectives
- Convey novel approach, expected outcomes, and team qualifications
- NSF: 1 page (Overview, Intellectual Merit, Broader Impacts sections); NIH: 30 lines

### Specific Aims (NIH) / Objectives
- Opening paragraph: knowledge gap and significance
- Central hypothesis, 2-4 independent but complementary aims
- Each aim: action-verb statement, rationale, working hypothesis, approach summary
- Payoff paragraph: transformative impact
- See `references/specific_aims_guide.md`

### Research Strategy / Project Description
- **Significance**: why the problem matters, gaps in current knowledge
- **Innovation**: what is novel (conceptual, methodological, integrative, translational)
- **Approach**: detailed methods, preliminary data, alternative strategies, rigor measures
- Agency-specific structure varies -- see reference files

### Broader Impacts (NSF) / Significance (NIH)
NSF evaluates broader impacts equally with intellectual merit. Address concrete activities in:
- Education/training integration
- Broadening participation of underrepresented groups
- Research infrastructure enhancement
- Public dissemination and outreach
- Societal benefits
- See `references/broader_impacts.md`

### Budget
Standard categories: Personnel, Equipment (>$5K), Travel, Materials/Supplies, Other Direct, F&A, Subawards

Agency-specific rules:
- **NSF**: Full justification, up to 2 months summer salary, no cost sharing required
- **NIH**: Modular ($250K increments) for standard R01; salary cap applies
- **DOE**: Often requires cost sharing; quarterly breakdown
- **DARPA**: Phase/task breakdown; cost-plus or firm-fixed-price structures
- See `references/budget_preparation.md`

## Review Criteria

### NSF
- Intellectual Merit: advance knowledge? well-conceived? qualified team?
- Broader Impacts: benefit society? meaningful broader impacts?

### NIH (scored 1-9, 1=exceptional)
Significance, Investigator(s), Innovation, Approach, Environment

### DARPA
- What if you succeed? What if you're right? Who cares?
- Technical merit, mission contribution, transition capability, cost realism

See `references/review_criteria.md` for full criteria.

## Common Proposal Types

**NSF**: Standard, CAREER (early career), Collaborative, RAPID (urgent, $200K), EAGER (high-risk, $300K)

**NIH**: R01 ($250K+/yr, 3-5yr), R21 (exploratory, $275K/2yr), R03 (small, $100K/2yr), F31/F32 (fellowships), K99/R00 (independence pathway)

**DOE**: Office of Science, ARPA-E, EERE, National Lab collaborations

**DARPA**: Program-specific BAAs, YFA (early career, $500K), Director's Fellowship

See `references/funding_mechanisms.md`

## Writing Principles

- **Narrative arc**: Hook → Problem → Solution → Evidence → Impact → Team
- **Multiple audiences**: technical reviewers, adjacent-field panelists, program officers
- **Active voice**, strong verbs, precise language
- **Figures**: self-explanatory with complete captions; generate with scientific-schematics skill
- **Risk management**: acknowledge challenges, provide alternatives, show preliminary data reducing risk
- **Coherence**: budget supports activities, timeline matches aims, team matches expertise needs

## Resubmission

**NIH A1**: 1-page introduction summarizing criticisms and specific changes. Address every major point. 37-month window.

**NSF**: Address reviewer concerns in revised proposal. No formal resubmission introduction.

See `references/resubmission_strategies.md`

## Resources

**Reference files** (load as needed):
`references/nsf_guidelines.md`, `references/nih_guidelines.md`, `references/doe_guidelines.md`, `references/darpa_guidelines.md`, `references/broader_impacts.md`, `references/specific_aims_guide.md`, `references/budget_preparation.md`, `references/review_criteria.md`, `references/timeline_planning.md`, `references/team_building.md`, `references/resubmission_strategies.md`

**Templates**: `assets/nsf_project_summary_template.md`, `assets/nih_specific_aims_template.md`, `assets/timeline_gantt_template.md`, `assets/budget_justification_template.md`, `assets/biosketch_templates/`

**Scripts**: `scripts/compliance_checker.py` (formatting), `scripts/budget_calculator.py` (budgets), `scripts/deadline_tracker.py` (deadlines)

