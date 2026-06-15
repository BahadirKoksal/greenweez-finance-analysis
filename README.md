# 📊 Greenweez Finance Analysis

> **Goal:** Monitor financial health and profitability daily for Greenweez e-commerce operations.

![Dashboard Preview](Grenweez%20Finance.png)

---

## 🧩 Project Overview

Greenweez's finance team needed a daily view of the company's financial performance. This project analyzes order-level data across revenue, costs, and ad spend to track key profitability KPIs and surface actionable insights.

---

## 📁 Data Sources

Three raw datasets were imported into Google Sheets using `IMPORTRANGE()`:

| Sheet | Contents |
|---|---|
| **Orders** | `datetime`, `orders_id`, `turnover`, `ship_fee`, `purchase_cost` |
| **Shipping** | `date_date`, `orders_id`, `log_cost`, `ship_cost` |
| **Campaigns** | `datetime`, `cost` (daily ad spend) |

---

## 🔧 Data Preparation & Enrichment

In the **Explo_Orders** sheet, raw order data was enriched with calculated KPI columns:

- `date_date` — cleaned date extracted from datetime using `SUBSTITUTE()`
- `gross_margin` — `turnover + ship_fee - purchase_cost`
- `gross_margin_percent` — `gross_margin / (turnover + ship_fee)`  
- `log_cost` — pulled from Shipping sheet via `XLOOKUP()`
- `ship_cost` — pulled from Shipping sheet via `XLOOKUP()`
- `operating_margin` — `gross_margin - (log_cost + ship_cost)`
- `operating_margin_percent` — `operating_margin / (turnover + ship_fee)`

In the **Explo_Campaigns** sheet, campaign data was enriched with:

- `date_date`, `year`, `month`, `day` — extracted using `LEFT()`, `YEAR()`, `MONTH()`, `DAY()`

---

## 📐 KPIs Tracked

| KPI | Formula | Purpose |
|---|---|---|
| **Gross Margin** | Turnover + Ship Fee − Purchase Cost | Controls impact of COGS |
| **Operating Margin** | Gross Margin − Operating Costs | Controls impact of operating expenses |
| **Ads Margin** | Operating Margin − Ads Cost | Overall profitability picture per product |

---

## 📊 Analysis & Dashboard

A **Daily Aggregation pivot table** was built from Explo_Orders, summarizing per day:

- Count of orders
- Sum & Average of turnover
- Sum of ship fees, purchase cost, gross margin, log cost, ship cost, operating margin
- Average of gross margin %, operating margin %

---

## 💡 Key Findings

- **Average ads margin per order: ~15.82%**
- Ad spend directly impacts profitability — on high-cost days (e.g. Oct 5 & 13), ads margin drops to ~12–13%, while on lower-spend days (e.g. Oct 4 & 6) it rises above 17%
- **Gross margins are stable** (~25€ per order), but **operating margins fluctuate** due to logistics and ad costs
- **High revenue ≠ high profitability** — Oct 10 had the highest turnover but only ~15.8% ads margin, while Oct 6 achieved a better margin (17.9%) with lower revenue

---

## 🛠️ Tools & Functions Used

| Function | Usage |
|---|---|
| `IMPORTRANGE()` | Import data from 3 external Google Sheets sources |
| `XLOOKUP()` | Bring log_cost and ship_cost from Shipping sheet by orders_id |
| `SUBSTITUTE()` | Clean datetime field — remove "00:00:00UTC" to get plain date |
| `LEFT()`, `YEAR()`, `MONTH()`, `DAY()` | Parse date components for time-based analysis |
| Formulas | Calculate KPIs (gross margin, operating margin) row by row |
| Pivot Table | Aggregate daily KPIs — COUNT, SUM, AVERAGE in one view |

---
