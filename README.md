# Renewable Energy KPI Dashboard

**End-to-end BI project** tracking methane recovery, uptime, CO₂ avoidance, and revenue across 5 landfill gas (LFG) processing facilities — built with Python (Pandas), DAX, and Power BI.

> **Stack:** Python · Pandas · Power BI · DAX · Power Query · CSV  
> **Period:** Jan 2023 – Dec 2024 · 5 Facilities · 24-Month Trend

---

## Dashboard Preview

![Dashboard Preview](dashboard/dashboard_preview.png)

Open `dashboard/dashboard_preview.html` in any browser for the interactive HTML version.

---

## Project Structure

```
renewable-energy-kpi-dashboard/
├── data/
│   ├── renewable_energy_kpi.csv       # Raw monthly KPI data (120 rows)
│   ├── daily_methane_recovery.csv     # Daily methane flow — 90 days (450 rows)
│   └── processed/
│       ├── kpi_clean.csv              # Transformed + derived KPIs (Power BI input)
│       ├── facility_summary.csv       # Facility-level aggregates
│       └── monthly_trend.csv          # 24-month trend series
├── python/
│   ├── generate_data.py               # Synthetic LFG dataset generator
│   └── etl_pipeline.py                # Extract → Transform → Load pipeline
├── dax/
│   └── measures.dax                   # All Power BI DAX measures (copy-paste ready)
├── dashboard/
│   └── dashboard_preview.html         # Interactive HTML dashboard (Chart.js)
└── README.md
```

---

## KPIs Tracked

| KPI | Description | Target |
|-----|-------------|--------|
| Methane Recovery % | % of landfill methane captured | ≥ 80% |
| Facility Uptime % | Operational availability | ≥ 88% |
| Gas Volume (MMBtu) | Total energy produced per month | — |
| CO₂ Avoided (tonnes) | Greenhouse gas offset | — |
| Revenue (USD) | Gas sales revenue | — |
| Efficiency Score | Composite score (recovery + uptime + revenue/MMBtu) | ≥ 85 |
| Compliance Status | Methane recovery ≥ 80% threshold | Compliant |

---

## Quick Start

### 1. Generate data
```bash
pip install pandas numpy
python python/generate_data.py
```

### 2. Run ETL pipeline
```bash
python python/etl_pipeline.py
```
Outputs land in `data/processed/` — connect Power BI to `kpi_clean.csv`.

### 3. Load into Power BI
1. Open Power BI Desktop → **Get Data → Text/CSV** → select `data/processed/kpi_clean.csv`
2. Open **DAX Studio** or the **Modeling** tab and paste measures from `dax/measures.dax`
3. Build visuals: line chart (methane trend), bar chart (facility revenue), matrix (compliance table)

---

## DAX Measures (highlights)

```dax
-- Rolling 3-month average methane recovery
[Methane Recovery 3M Rolling Avg] =
AVERAGEX(
    DATESINPERIOD( KPI[month], LASTDATE( KPI[month] ), -3, MONTH ),
    [Avg Methane Recovery %]
)

-- Month-over-month revenue change
[Revenue MoM Change %] =
VAR CurrentMonth  = MAX( KPI[month] )
VAR PreviousMonth = EDATE( CurrentMonth, -1 )
RETURN
DIVIDE(
    CALCULATE( [Total Revenue (USD)], KPI[month] = CurrentMonth ) -
    CALCULATE( [Total Revenue (USD)], KPI[month] = PreviousMonth ),
    CALCULATE( [Total Revenue (USD)], KPI[month] = PreviousMonth ), BLANK()
) * 100
```

Full set of 18 measures in [`dax/measures.dax`](dax/measures.dax).

---

## Key Findings (from sample data)

- **Westview Processing** leads in efficiency (90.9 score) and methane recovery (92.8%)
- **Riverside Energy Plant** is the only non-compliant facility (76.97% recovery < 80% threshold)
- Methane recovery improved **+2.7 percentage points** from Jan 2023 to Dec 2024
- Portfolio generated **$3.67M total revenue** and avoided **5,012 tonnes CO₂** over 24 months

---

## Author

**Shiva Ramakrishna Bandi** — BI Analyst  
[linkedin.com/in/srkbandi](https://linkedin.com/in/srkbandi)
