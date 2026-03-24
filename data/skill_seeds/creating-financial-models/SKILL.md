---
name: creating-financial-models
description: "Use when the user needs to build financial models for investment decisions, valuations, or risk assessment. Provides DCF analysis, sensitivity testing, Monte Carlo simulations, and scenario planning with complete financial projections, confidence intervals, and decision-support outputs in Excel format."
---

# Financial Modeling Suite

Build and analyze financial models for investment analysis, valuation, and risk assessment using industry-standard methodologies.

## Workflow

1. **Define model scope** — Identify model type (DCF, sensitivity, Monte Carlo, scenario) and gather required inputs
2. **Validate inputs** — Confirm financial statements, assumptions, and parameters are complete and consistent
3. **Build model** — Run `dcf_model.py` or `sensitivity_analysis.py` with validated inputs
4. **Run quality checks** — Verify balance sheet balancing, cash flow reconciliation, and statistical validation
5. **Generate outputs** — Produce projections, valuations, charts, and Excel workbook with full model

## Core Capabilities

### DCF Analysis
- Build complete DCF models with multiple growth scenarios
- Calculate terminal values using perpetuity growth and exit multiple methods
- Determine WACC and generate enterprise/equity valuations

**Required inputs**: Historical financials (3-5 years), growth assumptions, margin projections, capex forecasts, working capital, terminal growth rate, discount rate components

### Sensitivity Analysis
- Test key assumptions impact on valuation with data tables
- Generate tornado charts for sensitivity ranking and break-even analysis

**Required inputs**: Base case model, variable ranges, key metrics to track

### Monte Carlo Simulation
- Run 1,000-10,000 scenario iterations with probability distributions
- Generate confidence intervals (90%, 95%) and risk metrics (VaR, probability of loss)

**Required inputs**: Probability distributions for uncertain variables, correlation assumptions, iteration count

### Scenario Planning
- Build best/base/worst case scenarios with probability weights
- Compare outcome probabilities and risk-return profiles

**Required inputs**: Scenario definitions, probability weights, KPIs to track

## Model Types Supported

- **Corporate Valuation**: Mature companies, growth companies, turnarounds
- **Project Finance**: Infrastructure, real estate, energy projects
- **M&A Analysis**: Acquisition valuations, synergy modeling, accretion/dilution
- **LBO Models**: Leveraged buyouts, IRR/MOIC analysis, debt capacity

## Quality Checks

The model automatically performs:
1. Balance sheet balancing checks
2. Cash flow reconciliation
3. Circular reference resolution
4. Sensitivity bound checking
5. Statistical validation of Monte Carlo results

## Scripts

- `dcf_model.py`: Complete DCF valuation engine
- `sensitivity_analysis.py`: Sensitivity testing framework
