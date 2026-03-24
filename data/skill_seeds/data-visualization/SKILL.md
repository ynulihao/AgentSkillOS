---
name: data-visualization
description: "Use when creating charts, graphs, plots, dashboards, or any data visualization. Builds effective visualizations using Matplotlib, Seaborn, Plotly, D3.js, or Streamlit with focus on clarity, accessibility, and communicating data insights."
allowed-tools: Read, Grep, Glob, Edit, Write, Bash
---

# Data Visualization

Create data visualizations that communicate insights clearly. Covers chart selection, design principles, and implementation across Python and JavaScript libraries.

## Workflow

1. **Analyze data** - Understand structure, types, key metrics, dimensions, and the story to tell
2. **Select chart type** - Match visualization to data relationship (see selection guide below)
3. **Implement** - Build with appropriate library and apply design principles
4. **Validate** - Check labels, accessibility, color contrast, and that insights are clear
5. **Iterate** - Refine based on feedback and real data testing

## Chart Selection Guide

| Data Relationship | Chart Types |
|-------------------|-------------|
| Comparison | Bar, Grouped Bar, Bullet |
| Distribution | Histogram, Box Plot, Violin |
| Composition | Pie/Donut (< 6 categories), Stacked Bar, Treemap |
| Relationship | Scatter, Bubble, Heatmap |
| Time Series | Line, Area, Candlestick (OHLC) |
| Geographic | Choropleth, Point Map, Flow Map |

## Design Principles

- **Right chart for data**: Match visualization to data type and audience
- **Less is more**: Remove chartjunk and unnecessary elements
- **Clear labels**: Descriptive titles, axis labels, and legends
- **Accessible colors**: Use colorblind-friendly palettes; test with contrast checkers
- **Context**: Include reference points, baselines, and annotations
- **Interactive when helpful**: Tooltips, filters, and drill-downs for complex data

## Example: Matplotlib/Seaborn Dashboard

```python
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd

plt.style.use('seaborn-v0_8-whitegrid')
fig, axes = plt.subplots(2, 2, figsize=(14, 10))

# Line chart - time series with target comparison
ax1 = axes[0, 0]
ax1.plot(dates, revenue, marker='o', linewidth=2, label='Actual')
ax1.plot(dates, target, linestyle='--', linewidth=2, label='Target')
ax1.fill_between(dates, revenue, target, alpha=0.3,
                  where=(revenue >= target), color='green')
ax1.set_title('Revenue vs Target', fontweight='bold')
ax1.legend()

# Horizontal bar - category comparison
ax2 = axes[0, 1]
bars = ax2.barh(products, sales, color=sns.color_palette("Blues_r", n))
ax2.bar_label(bars, padding=3, fmt='$%.0fK')

# Scatter with regression - correlation
sns.regplot(data=df, x='ad_spend', y='conversions', ax=axes[1, 0])

# Donut chart - composition
axes[1, 1].pie(traffic, labels=channels, autopct='%1.1f%%',
               wedgeprops=dict(width=0.5))

plt.tight_layout()
plt.savefig('dashboard.png', dpi=150, bbox_inches='tight')
```

## Example: Interactive Plotly Chart

```python
import plotly.graph_objects as go

fig = go.Figure()
fig.add_trace(go.Scatter(x=dates, y=values, mode='lines', name='Daily'))
fig.add_trace(go.Scatter(x=dates, y=ma_7, mode='lines', name='7-day MA',
                          line=dict(dash='dash')))
fig.update_layout(
    title='Performance with Moving Average',
    hovermode='x unified', template='plotly_white',
    xaxis=dict(rangeselector=dict(buttons=[
        dict(count=7, label="1w", step="day", stepmode="backward"),
        dict(count=1, label="1m", step="month", stepmode="backward"),
        dict(step="all")
    ]), rangeslider=dict(visible=True))
)
fig.write_html('interactive_chart.html')
```

## Example: Streamlit Dashboard

```python
import streamlit as st

st.set_page_config(page_title="Dashboard", layout="wide")
col1, col2, col3 = st.columns(3)
col1.metric("Revenue", "$1.2M", "+12%")
col2.metric("Orders", "8,543", "+8%")
col3.metric("Conversion", "3.2%", "-0.5%")

with st.sidebar:
    date_range = st.date_input("Date Range", [])
    region = st.multiselect("Region", ["North", "South", "East", "West"])
```

## Validation Checklist

- [ ] Chart type matches the data relationship
- [ ] Axes labeled with units
- [ ] Title is descriptive
- [ ] Colors are colorblind-accessible
- [ ] No misleading scales (truncated axes, dual axes without clear labels)
- [ ] Legend present when multiple series shown
- [ ] Interactive elements work correctly (tooltips, filters)
