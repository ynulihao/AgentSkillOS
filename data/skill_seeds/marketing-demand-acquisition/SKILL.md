---
name: marketing-demand-acquisition
description: "Multi-channel demand generation, paid media optimization, SEO strategy, and partnership programs for Series A+ B2B SaaS startups. Use when planning demand generation campaigns, optimizing paid media (LinkedIn, Google, Meta ads), building SEO strategies, establishing partnerships, calculating CAC, setting up HubSpot campaigns, or expanding internationally. Triggers on: demand gen, paid ads, LinkedIn ads, Google ads, CAC, acquisition, lead generation, pipeline generation, MQL, SQL, or performance marketing."
---

# Marketing Demand & Acquisition

Expert acquisition playbook for Series A+ B2B SaaS startups scaling internationally (EU/US/Canada) with hybrid PLG/Sales-Led motion.

## Workflow

1. **Assess current state** - Identify funnel stage, channels in use, current CAC, and target metrics
2. **Select channel mix** - Use Channel Strategy Matrix below to prioritize based on ACV and audience
3. **Plan campaign** - Build campaign brief with objectives, budget, audience, offer, and success metrics
4. **Set up tracking** - Configure HubSpot campaign, UTM parameters, lead scoring, and attribution
5. **Execute and optimize** - Launch campaigns, monitor daily, apply scaling rules
6. **Report and iterate** - Build dashboards, run A/B tests (ICE score prioritization), adjust budget allocation

## Core KPIs

| Role | Primary KPIs |
|------|-------------|
| Demand Gen | MQL/SQL volume, cost per opportunity, marketing-sourced pipeline $, pipeline velocity |
| Paid Media | CAC, ROAS, CPL, CPA, incrementality lift |
| SEO | Organic sessions, non-brand traffic %, keyword rankings, organic-assisted conversions |
| Partnerships | Partner-sourced pipeline $, partner CAC, net new logos via partners |

## Channel Strategy Matrix

| Channel | Best For | CAC Benchmark | CVR | Series A Priority |
|---------|----------|---------------|-----|-------------------|
| LinkedIn Ads | B2B, Enterprise, ABM | $150-$400 | 0.5-2% | Highest |
| Google Search | High-intent, BOFU | $80-$250 | 2-5% | Highest |
| Google Display | Retargeting, awareness | $50-$150 | 0.3-1% | Medium |
| Meta (FB/IG) | SMB, ACV <$10k | $60-$200 | 1-3% | Medium |
| Partnerships | Co-marketing, referrals | $100-$300 | 5-10% | High |

## Full-Funnel Tactics

**TOFU (Awareness)**: Paid social thought leadership, display/programmatic, content syndication, SEO informational keywords, co-webinars. Target: brand lift, site traffic.

**MOFU (Consideration)**: Paid search solution keywords, retargeting, gated content, email nurture, comparison pages. Target: MQLs, demo requests.

**BOFU (Decision)**: Paid search brand/competitor keywords, free trial CTAs, case studies, intent-based retargeting. Target: SQLs, pipeline $.

## Campaign Brief Template

```
Campaign Name: [Q2-2025-LinkedIn-ABM-Enterprise]
Objective: [Generate 50 SQLs from Enterprise accounts ($50k+ ACV)]
Budget: [$15k/month] | Duration: [90 days]
Channels: [LinkedIn Ads, Retargeting, Email]
Audience: [Director+ at SaaS companies, 500-5000 employees, EU/US]
Success Metrics: Primary: 50 SQLs, <$300 CPO | Secondary: 500 MQLs, 10% MQL→SQL
UTM: utm_source={channel}&utm_medium={type}&utm_campaign={campaign-id}&utm_content={variant}
```

## Budget Allocation (Series A, $40k-60k/month)

- 40% Paid Acquisition (LinkedIn + Google)
- 25% Content/SEO
- 20% Partnerships
- 10% Tools/Automation
- 5% Experiments/Testing

**Scaling Rules**: CAC < target → increase 20% weekly. CAC > target → pause, optimize, relaunch. CVR drops >20% → check landing page, offer fatigue. 2-week minimum test period.

## International Considerations

**EU**: GDPR double opt-in, localize DE/FR/ES, LinkedIn primary channel, 40% LinkedIn / 25% Google / 20% SEO / 15% Partnerships.

**US/Canada**: Direct ROI-focused messaging, Google Ads + LinkedIn equal priority, faster sales cycles, 35% Google / 30% LinkedIn / 20% SEO / 15% Partnerships.

## MQL → SQL Handoff

**SQL Criteria**: Director+ title, 50-5000 employees, $10k+ budget, buying within 90 days, demo requested or high-intent action.

**SLA**: SDR responds to MQL within 4 hours → BANT qualification → if qualified, assign to AE within 24 hours → first demo within 3 business days.

## A/B Testing (ICE Score)

**Formula**: ICE = (Impact x Confidence x Ease) / 3 (each rated 1-10)

Prioritize tests with ICE > 7. Run 4-6 tests/month. Minimum 1000 visitors/variant, 95% confidence. Document losers for learnings.

## Detailed Playbooks

For channel-specific implementation details, consult:
- **references/hubspot-workflows.md** - Lead scoring, nurture sequences, assignment workflows
- **references/campaign-templates.md** - LinkedIn, Google, Meta campaign structures and ad copy frameworks
- **references/international-playbooks.md** - EU, US, Canada market entry tactics
- **references/attribution-guide.md** - Multi-touch attribution setup and analysis

## Scripts

- **scripts/calculate_cac.py** - Calculate blended and channel-specific CAC
- **scripts/experiment_calculator.py** - A/B test sample size and significance calculator
