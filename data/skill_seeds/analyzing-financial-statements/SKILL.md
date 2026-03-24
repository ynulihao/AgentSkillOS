---
name: analyzing-financial-statements
description: "Use when the user provides financial statement data and needs ratio analysis, benchmarking, or performance evaluation. Calculates profitability, liquidity, leverage, efficiency, and valuation ratios from income statements, balance sheets, and cash flow statements, then delivers interpreted results with industry context."
---

# Financial Ratio Calculator

Calculate and interpret key financial ratios from statement data for investment analysis and company evaluation.

## Workflow

1. **Collect financial data** — Accept input as CSV, JSON, text, or Excel containing income statement, balance sheet, and/or cash flow data
2. **Validate completeness** — Confirm required line items are present for requested ratios; flag missing values
3. **Calculate ratios** — Run `calculate_ratios.py` against the validated data
4. **Interpret results** — Run `interpret_ratios.py` to benchmark against industry standards and generate insights
5. **Deliver output** — Present calculated ratios, benchmarks, trend analysis (if multi-period), and formatted Excel report

## Ratio Categories

- **Profitability**: ROE, ROA, Gross Margin, Operating Margin, Net Margin
- **Liquidity**: Current Ratio, Quick Ratio, Cash Ratio
- **Leverage**: Debt-to-Equity, Interest Coverage, Debt Service Coverage
- **Efficiency**: Asset Turnover, Inventory Turnover, Receivables Turnover
- **Valuation**: P/E, P/B, P/S, EV/EBITDA, PEG
- **Per-Share**: EPS, Book Value per Share, Dividend per Share

## Output Format

- Calculated ratios with values
- Industry benchmark comparisons (when available)
- Trend analysis (if multiple periods provided)
- Interpretation and insights
- Excel report with formatted results

## Scripts

- `calculate_ratios.py`: Main calculation engine for all financial ratios
- `interpret_ratios.py`: Provides interpretation and benchmarking

## Validation Checkpoints

1. Verify data completeness before calculations — handle missing values with industry averages or exclusion
2. Cross-check that balance sheet balances (assets = liabilities + equity)
3. Consider industry context when interpreting ratios — not all ratios apply to all industries
4. Flag unusual or concerning ratios for further review
5. Include period comparisons for trend analysis when multi-period data is available
