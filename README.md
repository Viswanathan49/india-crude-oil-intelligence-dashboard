# India Crude Oil Intelligence Dashboard

An interactive Power BI dashboard analyzing India's crude oil import dependency over a decade (FY2014–FY2025), covering import trends, supply source diversification, refinery capacity utilization, and petroleum stock levels.

Built as a Capstone Project under the **Microsoft Elevate × AICTE Internship** program.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Excel](https://img.shields.io/badge/Microsoft%20Excel-217346?style=for-the-badge&logo=microsoftexcel&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=powerbi&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)

---

## Dashboard Preview

### Page 1 — Overview
> KPI cards, import vs. domestic production gap, crude oil import sources (donut chart), import dependency gauge, and key takeaways.

![Overview](1.png)

### Page 2 — Import Trends
> Decade-long import growth (bar chart), import volume vs. import bill comparison, import dependency trend (FY2014–FY2025), and key events timeline.

![Import Trends](2.png)

### Page 3 — Supply Sources
> Country-wise import share (stacked bar), Russia's dramatic rise as India's #2 supplier, regional import split (donut chart), and Indian basket crude price trend.

![Supply Sources](3.png)

### Page 4 — Refinery & Stocks
> Petroleum product stock levels (gauges), refinery utilization by company (bar chart), total installed refining capacity (258 MMTPA), and total supply vs. imports trend.

![Refinery & Stocks](4.png)

---

## Key Findings

| Metric | Value |
|--------|-------|
| Import Dependency | **89.2%** — one of the highest globally |
| Total Imports (FY2025) | **~240 MMT**, up from 189 MMT in FY2014 |
| Top 3 Suppliers | Iraq, Saudi Arabia, Russia |
| Russia's Rise | Became #2 supplier post-2022 due to geopolitical shifts |
| Domestic Production | Declined from 38 MMT to 29 MMT over the decade |
| Daily Consumption | **5+ MBPD** — 3rd largest globally |
| Refining Capacity | **258 MMTPA** across 23 refineries |
| Avg Crude Oil Price | **$85.3/bbl** (Indian Basket) |

---

## Data Model

The dashboard is built on a **star schema** with 6 interconnected tables linked via an `FY_Lookup` dimension table:

```
                    ┌─────────────────┐
                    │    FY_Lookup    │
                    │  (Dimension)    │
                    └────────┬────────┘
                             │
          ┌──────────────────┼──────────────────┐
          │                  │                  │
┌─────────┴──────┐ ┌────────┴────────┐ ┌───────┴────────┐
│ Crude_Yearly   │ │ Crude_Country   │ │ Refinery_Data  │
│ Extended       │ │ (Country-wise   │ │ (Throughput &   │
│ (Import/Prod)  │ │  imports)       │ │  Utilization)   │
└────────────────┘ └─────────────────┘ └────────────────┘
          │                  │                  │
┌─────────┴──────┐ ┌────────┴────────┐ ┌───────┴────────┐
│ Daily          │ │ Stock_Levels    │ │ Refining       │
│ Consumption    │ │ (Monthly stock  │ │ Capacity_2025  │
│ (POL demand)   │ │  percentages)   │ │ (23 refineries)│
└────────────────┘ └─────────────────┘ └────────────────┘
```

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Visualization | Microsoft Power BI Desktop |
| Data Transformation | Power Query (M Language) |
| Calculated Measures | DAX (Data Analysis Expressions) |
| Data Source Format | Excel (.xlsx), CSV |
| Theme | Custom JSON dark theme |
| Data Source | PPAC, Government of India |

---

## Project Structure

```
india-crude-oil-intelligence-dashboard/
│
├── dashboard/
│   ├── India_Crude_Oil_Dashboard.pbix       # Power BI report file
│   └── dark-theme.json                      # Custom dark theme
│
├── data/
│   ├── India_Crude_Oil_PowerBI_Data.xlsx     # Primary dataset (6 tables)
│   ├── India_Energy_Data.xlsx                # Extended energy data
│   ├── Refining-capacity-April-2025.xlsx     # Refinery capacity data
│   └── domestic_lpg_prices_historical_data.csv
│
├── screenshots/
│   ├── overview.jpg                         # Page 1 — Overview
│   ├── import-trends.jpg                    # Page 2 — Import Trends
│   ├── supply-sources.jpg                   # Page 3 — Supply Sources
│   └── refinery-stocks.jpg                  # Page 4 — Refinery & Stocks
│
├── references/
│   └── PPAC_Monthly_Reports/                # Reference PDFs from PPAC
│
├── LICENSE
└── README.md
```

---

## Dataset Details

### Primary Dataset — `India_Crude_Oil_PowerBI_Data.xlsx`

| Table | Records | Description |
|-------|---------|-------------|
| `Crude_Yearly_Extended` | 12 rows | Yearly import volumes, domestic production, prices, and consumption (FY2014–FY2025) |
| `Crude_Country` | 56+ rows | Country-wise crude oil import volumes and market share (FY2018–FY2025) |
| `Refinery_Data` | 160+ rows | Refinery-wise throughput, capacity, and utilization rates |
| `Stock_Levels` | 60+ rows | Monthly petroleum product buffer stock levels (LPG, Petrol, Diesel, Crude, ATF) |
| `Refining_Capacity_2025` | 23 rows | Current installed refining capacity by refinery (as of April 2025) |
| `Daily_Consumption` | 8 rows | Annual POL consumption and daily processing rates |

---

## DAX Measures (Key Calculations)

```dax
-- Import Dependency Ratio
Import Dependency % = 
    DIVIDE([Total_Imports_MMT], [Total_Supply_MMT]) * 100

-- Year-over-Year Growth
YoY Import Growth % = 
    VAR CurrentYear = [Total_Imports_MMT]
    VAR PreviousYear = CALCULATE([Total_Imports_MMT], PREVIOUSYEAR('FY_Lookup'[Date]))
    RETURN DIVIDE(CurrentYear - PreviousYear, PreviousYear) * 100

-- Country Share Percentage
Country Share % = 
    DIVIDE(
        SUM(Crude_Country[Import_Volume_MMT]),
        CALCULATE(SUM(Crude_Country[Import_Volume_MMT]), ALL(Crude_Country[Country]))
    ) * 100
```

---

## How to Use

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/india-crude-oil-intelligence-dashboard.git
   ```

2. **Open in Power BI Desktop**
   - Install [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
   - Open `dashboard/India_Crude_Oil_Dashboard.pbix`

3. **Apply the dark theme** (if not auto-applied)
   - Go to `View` → `Themes` → `Browse for themes`
   - Select `dashboard/dark-theme.json`

4. **Refresh data** (optional)
   - Go to `Home` → `Transform Data` to view Power Query steps
   - Click `Close & Apply` to reload

---

## Data Sources & References

| Source | URL | Data Used |
|--------|-----|-----------|
| Petroleum Planning and Analysis Cell (PPAC) | [ppac.gov.in](https://ppac.gov.in) | Crude oil imports, production, refinery data, consumption |
| PPAC — Import/Export Data | [ppac.gov.in/import-export](https://ppac.gov.in/import-export) | Country-wise crude oil import volumes |
| PPAC — Consumption Data | [ppac.gov.in/consumption/products-wise](https://ppac.gov.in/consumption/products-wise) | Product-wise petroleum consumption |
| PPAC — Refinery Capacity | [ppac.gov.in/infrastructure/installed-refinery-capacity](https://ppac.gov.in/infrastructure/installed-refinery-capacity) | Refinery-wise installed capacity |
| PPAC — Crude Processing | [ppac.gov.in/production/crude-processing](https://ppac.gov.in/production/crude-processing) | Refinery throughput data |
| Ministry of Petroleum & Natural Gas | [mopng.gov.in](https://mopng.gov.in) | Policy and supplementary data |
| Open Government Data Platform | [data.gov.in](https://data.gov.in/keywords/Crude%20Oil) | Crude oil datasets |
| NITI Aayog ICED | [iced.niti.gov.in](https://iced.niti.gov.in/analytics/india-crude-oil-prices-vs-import) | Crude oil prices vs import analysis |
| Energy Information Administration | [eia.gov](https://www.eia.gov) | International energy statistics |
| Indian Oil Corporation | [iocl.com](https://iocl.com) | Annual reports |

---

## Author

**Viswanathan S**
- Microsoft Elevate × AICTE Internship — Capstone Project
- Contact : viswanathansk49@gmail.com
---

## License

This project is licensed under the [MIT License](LICENSE).

---

> *"India imports 89.2% of its crude oil — understanding this dependency is not just an academic exercise, it's a matter of national energy security."*
