# Oil Market Report — Power BI Dashboard
### Global Oil Demand, Supply & Crisis Impact Analysis | IEA March 2026

---

## Overview

An end-to-end data analytics project analysing the global oil market 
using the IEA Oil Market Report (March 2026). The project covers the 
full analytics pipeline — from raw PDF extraction to a 4-page 
interactive Power BI dashboard — and includes the first major supply 
shock analysis since the Strait of Hormuz closure on February 28 2026.

---

## The Business Problem

Energy analysts, portfolio managers and policy teams need to quickly 
understand three things from the IEA monthly report:

1. Where is global oil demand growing and what is driving it?
2. How is supply responding — and which countries are leading?
3. What is the quantified impact of the Hormuz crisis on the market?

This dashboard answers all three questions in a single interactive report.

---

## Key Findings

| Finding | Data point |
|---|---|
| Global demand 2024 | 103.8 mb/d — slowest growth (+0.8%) since 2020 |
| Demand growth driver | Petrochemicals accounted for 68% of 2024 growth |
| Structural shift | Non-OECD now 56% of global demand (up from 43% in 2000) |
| Fastest growing country | India +3.4% in 2024 — largest single-country gain |
| Supply disruption | Hormuz closure removed 8 mb/d (7.7% of global supply) |
| Price impact | Brent rose from $71/bbl to $92/bbl in two weeks |
| Russia revenue | Fell to $9.5bn in Feb 2026 — lowest since Ukraine invasion |
| IEA response | 400 million barrel emergency stock release — March 11 2026 |

---

## Dashboard Structure

### Page 1 — Executive Overview
Global supply and demand trend 2022–2025 with KPI cards for total 
demand, year-on-year growth, supply gap and Brent price. OECD vs 
Non-OECD demand share donut. Top consuming countries ranked bar.

### Page 2 — Demand Deep Dive
What drove 2024 demand growth — waterfall chart by driver. Oil demand 
by product (2019 vs 2024) showing the petrochemical shift. Regional 
demand trend by key geography 2023–2025.

### Page 3 — Supply and Trade
Top 10 producing countries by volume (2024–2026). Russian export 
revenue trend Jan 2025–Feb 2026 with crisis annotation. OECD imports 
by product and region.

### Page 4 — Hormuz Crisis Impact *(showcase page)*
Crisis KPI cards — supply shut in, Brent spike, demand forecast cut, 
IEA stock release. Before vs after demand comparison by product. 
Interactive bookmark toggle switching between pre-crisis and 
post-crisis views.

---

## Tools and Technologies

| Stage | Tool | Purpose |
|---|---|---|
| Data extraction | Python (pdfplumber, openpyxl) | Extract tables from IEA OMR PDF |
| Data cleaning | Python (pandas) | Unpivot, remove totals, fix data types |
| Data modelling | Power BI Desktop | Star schema, DAX measures, relationships |
| Visualisation | Power BI Desktop | 4-page interactive report |
| Version control | GitHub | Project repository |

**Technical skills demonstrated:**
`Python` `pandas` `openpyxl` `Power BI` `DAX` `Power Query (M)` 
`SQL-style data modelling` `PDF data extraction` `ETL pipeline`

---

## Data Sources

| Table | Source | Content |
|---|---|---|
| T1 World Supply & Demand | IEA OMR March 2026, Table 1 | Supply/demand balance by category 2022–2026 |
| T2 Global Oil Demand | IEA OMR March 2026, Table 2 | Demand by region and selected countries |
| T3 World Oil Production | IEA OMR March 2026, Table 3 | Production by country 2024–2026 |
| T7 OECD Imports | IEA OMR March 2026, Table 7 | Import volumes by product and region |
| T18 Russian Exports | IEA OMR March 2026, Table 18 | Export destinations and revenue monthly |
| S3 By Product | IEA Oil 2024/2025, Global Energy Review 2025 | Demand by product type 2019–2024 |
| S4 Growth Drivers | IEA Global Energy Review 2025 | 2024 demand growth attribution |
| S4 Hormuz Outlook | IEA OMR March 2026 | 2025F vs 2026F demand by product |

**Primary source:** IEA Oil Market Report, 12 March 2026  
**Secondary sources:** IEA Global Energy Review 2025 · IEA Oil 2024/2025 · OPEC World Oil Outlook 2024

---

## Data Preparation Process

The raw IEA OMR arrives as a PDF with pivoted multi-header tables, 
embedded section labels and total rows — not usable directly in 
Power BI. The cleaning pipeline:

1. **Extract** — pdfplumber extracts all 8 tables from the 92-page PDF
2. **Strip** — metadata rows, blank separator rows and footnotes removed
3. **Label extraction** — section headers (OECD DEMAND, NON-OECD SUPPLY 
   etc.) converted from row labels to a Category dimension column
4. **Remove totals** — all aggregation rows removed so Power BI 
   calculates them dynamically
5. **Unpivot** — year columns melted from wide to long format 
   (one row per observation)
6. **Type fix** — Year column converted from string to integer for 
   DAX compatibility
7. **Enrich** — Source and Unit columns added for traceability

Full methodology documented in `python/Data_Preparation_Guide.docx`

---

## DAX Measures

Key measures built in Power BI:

```dax
-- Total demand (OECD + Non-OECD only, no double counting)
Total Demand =
CALCULATE(
    SUM('T1 World Supply Demand'[Value_mbd]),
    'T1 World Supply Demand'[Category] IN {"OECD DEMAND","NON-OECD DEMAND"}
)

-- Year-on-year growth %
Demand YoY% =
VAR CY = SELECTEDVALUE(CalendarTable[YEAR])
VAR Current = [Total Demand]
VAR Prior = CALCULATE([Total Demand],
    ALL(CalendarTable), CalendarTable[YEAR] = CY - 1)
RETURN DIVIDE(Current - Prior, Prior, BLANK())

-- Supply vs demand gap
Supply Gap = [Total Supply] - [Total Demand]
```

---

## How to Run

1. Download both `.xlsx` files from the `data/` folder
2. Open Power BI Desktop
3. Home → Get Data → Excel → load both files, select all sheets
4. Create relationships: all table `Year` columns → `CalendarTable[YEAR]`
5. Copy DAX measures from this README or rebuild following the 
   comments in the `.pbix` file
6. Open the `.pbix` file directly if provided

---

## About

**Meena Raja**  
Data and Reporting Specialist — 5+ years in banking and financial services  
Skills: SQL · Power BI · Snowflake · Python · DAX · Power Query · Azure DevOps

[LinkedIn](https://linkedin.com/in/meenar09) · [Email](mailto:meenar.2893@gmail.com)

---

*Data source: IEA Oil Market Report, March 2026. All analysis and 
interpretations are the author's own. This project is for portfolio 
and educational purposes.*
