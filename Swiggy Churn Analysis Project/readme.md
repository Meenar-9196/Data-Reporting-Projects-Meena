# 🍊 Swiggy Churn Analysis
### Data & Reporting Portfolio Project · FY2020 – Q1 FY2026

> **A full-cycle data analysis project** — from company selection and raw data collection through Python EDA, Looker Studio dashboards, and a 3Q storytelling insights report — examining why Swiggy lost customers and what they can do about it.

---

## 📌 Project Overview

| Item | Detail |
|------|--------|
| **Company** | Swiggy Limited (NSE/BSE: SWIGGY) |
| **Sector** | Food Delivery & Quick Commerce · India |
| **Scope** | FY2020 (Apr 2019) → Q1 FY2026 (Jun 2025) |
| **Analyst** | Meena · Data & Reporting Specialist |
| **Status** | ✅ Complete |
| **Tools used** | Excel · Python · Looker Studio · Word |

This project analyses Swiggy's customer churn patterns using 6 years of public data. The goal was to produce a credible, data-driven report that could be shared directly with Swiggy's data and product teams — not just a portfolio exercise, but a real analytical contribution.

---

## 🎯 Why Swiggy?

Swiggy is India's #2 food delivery platform with a **documented, publicly verifiable churn crisis**:
- Lost **107M+ subscribers** between FY2020 and FY2024
- Market share fell from **50% to 37%** between FY2020 and FY2023
- Only **7% of new users** are still active 4 months after their first order
- Consistently rated **1.5 / 5** across Trustpilot, PissedConsumer, and MouthShut
- Still **loss-making** (−₹3,117 Cr in FY2025) while competitor Zomato turned profitable

All data used in this project is **publicly available** from official filings, analyst reports, and verified review platforms.

### Step-by-step breakdown

| Step | Description | Output |
|------|-------------|--------|
| 1 | Company selection with web research | Swiggy selected |
| 2 | 5-year data collection (financial, users, market share, reviews) | Raw Excel file |
| 3 | Data cleaning — fix types, flags, snake_case headers | Cleaned CSV files |
| 4 | Exploratory Data Analysis (Python) | EDA notebook |
| 5 | Looker Studio dashboards (4 pages) | Published report |
| 6 | 3Q storytelling insights document | Word / PDF report |

---

## 📁 Repository Structure

```
swiggy-churn-analysis/
│
├── data/
│   ├── raw/
│   │   └── swiggy_data.xlsx        # Long-format raw data (6 sheets)
│   └── cleaned/
│       ├── financial_performance.csv
│       ├── user_customer_metrics.csv
│       ├── market_share_competitive.csv
│       └── customer_reviews_sentiment.csv
│
├── notebooks/
│   └── swiggy_data_cleaning.ipynb                   # Python EDA notebook
│
├── reports/
│   ├── Swiggy_Churn_Analysis_Report.pdf   # Final 3Q insights report
│   └── Dashboard.pdf       # Looker Studio dashboard export
│
├── looker_studio/
│   └── [dashboard_link.md](https://datastudio.google.com/reporting/64456cc1-ea3a-49bd-b533-74156a9ad61e)                  # Public Looker Studio link
│
└── README.md
```

---

## 📊 Data Sources

All data is sourced from **publicly available** information only.

| Source | Type | Confidence |
|--------|------|------------|
| Swiggy Annual Reports FY2020–FY2025 | Official filing | ✅ High |
| Swiggy DRHP / IPO Prospectus 2024 | SEBI official filing | ✅ High |
| Swiggy Q4 FY25 & Q1 FY26 Shareholder Letters | Official communication | ✅ High |
| Entrackr (RoC filings) | Financial data aggregator | ✅ High |
| Goldman Sachs market share research | Analyst research note | ✅ High |
| HSBC Global Research (Zomato Gold study) | Analyst research note | ✅ High |
| Bernstein analyst report (MTU estimates) | Analyst research note | 🟡 Medium |
| GrowthX Swiggy deep dive | Platform research | 🟡 Medium |
| CleverTap Benchmark Report 2024 | Industry benchmark | ✅ High |
| IRJET Nov 2025 (Zomato vs Swiggy study) | Academic paper | ✅ High |
| Trustpilot / PissedConsumer / MouthShut | Public reviews | ✅ Verified |
| Datum Intelligence / Mukund Mohan blog | Market share data | 🟡 Medium |
| Outlook Business / Business Standard | News / Q1 FY26 results | ✅ High |

> **Note:** Rows flagged `is_estimate = 1` in the raw Excel file are analyst estimates. All official filing rows are flagged `is_estimate = 0`. Always check this column before using any figure in a calculation.

---

## 🗂️ Excel Data Structure

The raw Excel file (`swiggy_data.xlsx`) in long/tidy format — one row per metric per time period. This format is designed for direct loading into Pandas.

| Sheet | Rows | Contents |
|-------|------|----------|
| `financial_performance` | 26 | Revenue, Net Loss, GOV by segment · FY2020–Q1 FY2026 |
| `user_customer_metrics` | 38 | Platform MTU, Food MTU, Instamart MTU, Cities, Partners, Retention |
| `market_share_competitive` | 31 | Market share · Zomato / Zepto benchmarks · AOV · Dark stores |
| `customer_reviews_sentiment` | 22 | Platform ratings · Complaint themes · Retention funnel · Quotes |
| `data_dictionary` | — | All column definitions, data types, usage warnings |
| `readme` | — | Project metadata, sources, audit log (v2 fixes) |

### Key columns

| Column | Type | Description |
|--------|------|-------------|
| `fiscal_year` | Text | Display label e.g. FY2020, Q1 FY2026 |
| `year_start` | Integer | Numeric start year — use for sorting and time-axis |
| `year_end` | Integer | Numeric end year — same as year_start for single-year rows |
| `sort_order` | Integer | 1–7 chronological · 99 for reference/benchmark rows |
| `metric_name` | Text | The metric being recorded |
| `metric_category` | Text | Higher-level grouping for filtering |
| `value` | Float | Numeric value (Sheets 1–3) |
| `value_numeric` | Float | Numeric value (Sheet 4 only — null for quote rows) |
| `value_text` | Text | Text value (Sheet 4 only — null for numeric rows) |
| `is_estimate` | Integer | 0 = official source · 1 = analyst estimate |
| `is_primary_company` | Integer | 1 = Swiggy · 0 = competitor (Sheet 3 only) |

---

## 🐍 Python Cleaning Notes

```python
import pandas as pd

# Load a sheet
df = pd.read_excel("data/raw/swiggy_raw_data_v2.xlsx",
                   sheet_name="financial_performance")

# Key patterns for cleaning
df["value"] = pd.to_numeric(df["value"], errors="coerce")
df_official   = df[df["is_estimate"] == 0]          # official data only
df_timeseries = df[df["sort_order"] < 99]           # exclude reference rows
df_swiggy     = df[df["company"] == "Swiggy"]       # Sheet 3 Swiggy-only filter

# For Sheet 4 (reviews)
df_reviews = pd.read_excel("...", sheet_name="customer_reviews_sentiment")
df_numeric = df_reviews[df_reviews["value_numeric"].notna()]   # numeric rows
df_quotes  = df_reviews[df_reviews["value_text"].notna()]      # quote rows
```

---

## 📈 Looker Studio Dashboard

The dashboard is built across **4 pages** in Looker Studio, connected via Google Sheets (cleaned CSVs):

| Page | Title | Key charts |
|------|-------|------------|
| 1 | Financial Performance | Revenue vs Net Loss combo · YoY growth · GOV stacked bar |
| 2 | Customer & Retention | MTU trend · Retention funnel · City growth |
| 3 | Competitive Intelligence | Market share line · Quick commerce donut · Revenue comparison |
| 4 | Customer Reviews & Sentiment | Complaint bar · Platform ratings · Voice of customer table |

🔗 **[https://datastudio.google.com/reporting/64456cc1-ea3a-49bd-b533-74156a9ad61e](#)**

---

## 📋 Key Findings

### Financial
- Revenue grew **339%** from ₹3,468 Cr (FY2020) to ₹15,227 Cr (FY2025)
- Net loss **peaked at ₹4,179 Cr in FY2023** — the worst year for both losses and market share
- Zomato turned **profitable in FY2025 (+₹527 Cr)** while Swiggy still lost ₹3,117 Cr

### Customer & Retention
- Only **7% of new users** are still active at month 4 (M+4 retention rate)
- Platform MTU fell **15.9%** from 17M (FY2023) to 14.7M (FY2024) despite revenue growth
- Swiggy has **112.7M lifetime users** but retains fewer than 20M monthly — an 82% lapsed user base

### Competitive
- Swiggy went from **market leader (50%) in FY2020 to 18pts behind Zomato (37%) in FY2023**
- Instamart holds only **25% of quick commerce** vs Blinkit's 46%
- Blinkit AOV is **₹138 higher per order** than Instamart

### Customer Sentiment
- Average rating of **1.5 / 5** across Trustpilot, PissedConsumer, and MouthShut
- Only **15%** of PissedConsumer reviewers would recommend Swiggy
- **34%** of complaints are delivery delays and false ETAs — the #1 churn trigger

---

## 💡 3Q Insight Summary

### Q1 — What happened?
Swiggy grew revenue 339% in 5 years but lost market share, lost customers, and remained loss-making. The defining signal: MTU fell 15.9% in FY2024 even as revenue grew — Swiggy was extracting more from fewer customers.

### Q2 — Why does it matter?
Swiggy is in a compounding churn loop: losses force cost cuts → cost cuts worsen service → poor service drives churn → churn forces more spending on acquisition → acquisition costs sustain losses. A 1.5/5 customer rating and 93% M+4 churn rate are not isolated problems — they are symptoms of the same structural failure.

### Q3 — Now what?
Three highest-priority actions:
1. **Fix ETA accuracy** — ML-based prediction eliminates the #1 complaint (34% of all reviews)
2. **Launch a First-Order Guarantee** — converts one-time users into habitual customers
3. **Upgrade Swiggy One loyalty** — the single highest-ROI retention lever, directly countering Zomato Gold



## ⚖️ Legal & Ethics Note

- All data is sourced from **publicly available** information only
- No internal Swiggy data, APIs, or proprietary information was used
- This is an **independent analysis** for portfolio and educational purposes
- Not affiliated with or commissioned by Swiggy Limited
- Customer review quotes are reproduced from public platforms under fair use for analytical commentary
