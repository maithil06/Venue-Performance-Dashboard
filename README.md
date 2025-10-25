# Venue Performance Dashboard (Power BI)

**Personal Project — Feb 2025**

Interactive BI dashboards for a theatre venue, analysing 12 months of operations (sales, profit, quantity, tills, and departments). Built with SQL + Power BI and designed for stakeholder self-service across seasons and locations.

---

## 🔍 What's Inside

- **KPI tiles:** Avg Order Value, Total Revenue, Total Quantity, Orders, Gross Profit, Total Pay, Avg Hours
- **Time views:** Sales/Profit by Day and Sales by Time of Day
- **Dimensional views:** Top Sales by Department, Top 5 Products (Quantity)
- **Slicers:** Till, Season, Venue/Bar (Company vs Drum Bar), Date Range
- **Hypothesis testing:** Track promo weeks, staffing level changes, and seasonality effects

---

## 📸 Screenshots

<p align="center">
  <img src="assets/dashboard-overview.png" width="800" alt="Sales Overview Dashboard">
  <br>
  <em>Sales Overview Dashboard</em>
</p>

<p align="center">
  <img src="assets/dashboard-profit.png" width="800" alt="Profit Overview Dashboard">
  <br>
  <em>Profit Overview Dashboard</em>
</p>

> **Note:** Place your images at `assets/dashboard-overview.png` and `assets/dashboard-profit.png` in the repo.

---

## 🧱 Tech Stack

- **Power BI** — Data model, DAX, report design
- **SQL** — Cleaning, joins, rollups
- **Power Query (M)** — ETL within Power BI
- **Git LFS** — To track `.pbix` (optional)

---

## 📊 Data Model (Star Schema)

- **Fact:** `FactSales` (date_key, product_key, till_id, qty, gross_sale, discount, net_value, cogs, gross_profit, order_number, hour)
- **Dimensions:** `DimDate`, `DimProduct` (dept, category), `DimTill`, `DimVenue`, `DimSeason`
- **Granularity:** Row per line-item sale

---

## 🧪 Hypothesis-Driven Metrics

- **Promo impact:** Compare promo vs non-promo weeks on AOV and GP%
- **Staffing impact:** Correlate Avg Hours vs throughput (Orders/Hour)
- **Seasonality:** Compare Dec Panto vs baseline months

---

## 📐 Key DAX (Selected)
```dax
-- Average Order Value (AOV)
AOV :=
DIVIDE( [Total Net Value], [Orders] )

Total Net Value :=
SUM ( FactSales[net_value] )

Orders :=
DISTINCTCOUNT ( FactSales[order_number] )

Gross Profit (GP) :=
SUM ( FactSales[gross_profit] )

GP % :=
DIVIDE( [Gross Profit (GP)], [Total Net Value] )

Sales by Time (Hour of Day) :=
VAR h = SELECTEDVALUE ( FactSales[hour] )
RETURN CALCULATE ( [Total Net Value], FactSales[hour] = h )

Distance per Minute (example calc pattern) :=
DIVIDE( [Total Distance (m)], [Play Time (min)] )
```

---

## 🧹 Example SQL (Pre-Power BI)
```sql
-- Daily aggregates (used for "Sales by Day" and KPI cards)
SELECT
  CAST(sale_datetime AS DATE) AS sale_date,
  COUNT(DISTINCT order_number) AS orders,
  SUM(quantity) AS qty,
  SUM(net_value) AS revenue,
  SUM(gross_profit) AS gross_profit
FROM raw_sales
WHERE sale_datetime BETWEEN '2024-05-01' AND '2025-04-30'
GROUP BY CAST(sale_datetime AS DATE);
```

---

## 📁 Repo Structure
```
.
├── assets/
│   ├── dashboard-overview.png
│   └── dashboard-profit.png
├── data_sample/
│   ├── FactSales_sample.csv
│   ├── DimDate.csv
│   └── DimProduct.csv
├── sql/
│   └── daily_aggregates.sql
├── powerbi/
│   └── Venue_Performance.pbix
└── README.md
```

---

## ▶️ Getting Started

1. Clone the repo and open `powerbi/Venue_Performance.pbix`
2. Swap the data source to your local/warehouse paths
3. Refresh; validate relationships: `FactSales` ⇄ `DimDate/DimProduct/DimTill/DimSeason`
4. Use the Till, Season, and Venue slicers to explore

---

## 📈 Example Insights (From Sample Data)

- Total Revenue ≈ **£122.5K** and Orders ≈ **10.28K** in the period shown
- **Top Depts by revenue:** Ice Creams, Wines, Soft Drinks, Lager/Beer
- **Time-of-day spikes** align with show intervals (pre-show & interval peaks)
- **Gross Profit concentration** in Wines and Beer supports targeted upsell

---

## 🗺️ Roadmap

- [ ] Staff-to-sales productivity KPI (Orders per Staff Hour)
- [ ] Cohort analysis by show type / event
- [ ] Price elasticity experiments
- [ ] Inventory linkage (stockouts & waste tracking)
- [ ] RLS for role-based venue access

---

## 📜 License

MIT — feel free to fork and adapt. If you use it, a star ⭐ is appreciated!

---

## 🙋‍♂️ Author

**Maithil Tandel** — ML/DS + Analytics

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue)](your-linkedin-url)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black)](your-github-url)
