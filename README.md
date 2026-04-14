# 🛒 Walmart Sales Analytics — End-to-End Data Analysis Project

> **Transforming 550K transactions into actionable business decisions using SQL, Python, and Power BI**

---

## 📌 Business Problem

Walmart's leadership needs to understand **why revenue has been flat for 4 years** despite high transaction volume, **which regions and categories are underperforming**, and **where the biggest growth opportunities lie** — across regions, store types, product categories, and customer segments.

This project answers 15 real business questions a data or strategy team would ask, from executive KPIs to customer lifetime value and employee performance.

---

## 🎯 Key Business Questions Answered

| # | Business Question | Technique Used |
|---|---|---|
| 1 | What is overall business performance? | Aggregate KPIs |
| 2 | Is revenue growing or declining month over month? | MoM Growth, LAG() |
| 3 | Which region and store type drives the most revenue? | GROUP BY + RANK() |
| 4 | Which product categories are gaining or losing share? | YoY Growth, window functions |
| 5 | Who are our highest lifetime value customers? | LTV ranking, RANKX |
| 6 | Which customer segment is most profitable? | Segment profitability |
| 7 | Which employees and departments outperform? | PARTITION BY dept |
| 8 | What payment method generates highest order value? | Payment analysis |
| 9 | Are there seasonal or day-of-week revenue patterns? | DOW analysis |
| 10 | What is the quarterly revenue breakdown? | Quarter-over-quarter |

---

## 💡 Top 5 Insights Discovered

**1. Revenue is flat — not growing**
Monthly revenue has stayed between $14M–$17M from 2020–2023 with no upward trend. Despite 550K transactions, no category grew more than 1% YoY. This signals a **saturation problem**, not a demand problem.

**2. Central region outperforms West by $59M**
Central ($221M) beats West ($162M) by 37% — the largest regional gap. Every product category shows a $12–13M Central-West gap. West is the single biggest untapped revenue opportunity.

**3. Accessories alone drive 53% of Clothing revenue**
Accessories ($90M) is 1.2× bigger than Smartphones and single-handedly props up the Clothing category. If accessories underperform, Clothing ($169M) collapses. High concentration risk.

**4. Consumer segment dominates but Corporate is underleveraged**
Consumer generates $519M vs Corporate's $153M. Yet Corporate customers have higher order values. The B2B segment is systematically underserved — a major growth lever.

**5. Employee performance is highly uneven**
Only 33 of 93 employees exceed the average revenue threshold. Top performer (Eve Smith, $18.5M) outperforms the lowest by 26%. Department-level coaching could unlock significant revenue.

---

## 🏗️ Project Architecture

```
walmart-sales-analytics/
│
├── data/                        # Raw and cleaned CSV files
│   ├── clean_customer.csv
│   ├── clean_product.csv
│   ├── clean_store.csv
│   ├── clean_employee.csv
│   └── clean_sales.csv
│
├── notebooks/
│   ├── Data_Cleaning.ipynb      # Pandas data cleaning pipeline
│   ├── EDA.ipynb                # Exploratory data analysis + visualizations
│   └── Connection_ETL.ipynb     # PostgreSQL ETL via SQLAlchemy
│
├── sql/
│   └── sql_business_analysis.sql  # 15 business queries with window functions
│
├── images/                      # Dashboard screenshots
│
├── dashboard.pbix               # Power BI report file
├── dashboard_pdf.pdf            # Static PDF export
└── README.md
```

---

## 🛠️ Tech Stack

| Layer | Tool | Purpose |
|---|---|---|
| Data Cleaning | Python, Pandas | Null handling, type casting, deduplication |
| Storage | PostgreSQL | Relational database for structured querying |
| ETL | SQLAlchemy, psycopg2 | Python → PostgreSQL pipeline |
| Analysis | SQL (PostgreSQL) | 15 business queries, window functions, CTEs |
| Visualization | Power BI | 3-page interactive dashboard |
| Version Control | Git, GitHub | Project tracking and portfolio sharing |

---

## 📊 Dashboard Overview

3-page interactive Power BI dashboard with cross-page slicers (Year, Region, Store Type):

**Page 1 — Executive Summary**
KPI cards · Monthly trend · Revenue by Region · Top States map · Key insights

**Page 2 — Product & Store**
Revenue by Category · Top 10 Sub-categories · YoY% performance table · Regional breakdown

**Page 3 — Customer & Employee**
Customer LTV ranking · Segment analysis · Age group breakdown · Employee performance

---

## 🔍 SQL Highlights

This project uses advanced SQL beyond basic SELECT queries:

```sql
-- MoM Revenue Growth using LAG() window function
WITH monthly_rev AS (
    SELECT
        EXTRACT(YEAR FROM "Sale_Date"::date) AS year,
        EXTRACT(MONTH FROM "Sale_Date"::date) AS month,
        ROUND(SUM("Total_Price")::numeric, 2) AS revenue
    FROM clean_sales
    GROUP BY year, month
)
SELECT
    year, month, revenue,
    ROUND(
        ((revenue - LAG(revenue) OVER (ORDER BY year, month))
         / LAG(revenue) OVER (ORDER BY year, month) * 100)::numeric, 2
    ) AS mom_growth_pct
FROM monthly_rev
ORDER BY year, month;
```

```sql
-- Customer LTV with segment ranking
SELECT
    c."Customer_Name", c."Segment",
    ROUND(SUM(s."Total_Price")::numeric, 2) AS lifetime_value,
    RANK() OVER (ORDER BY SUM(s."Total_Price") DESC) AS ltv_rank,
    RANK() OVER (PARTITION BY c."Segment" ORDER BY SUM(s."Total_Price") DESC) AS rank_in_segment
FROM clean_sales s
JOIN clean_customer c ON s."Customer_ID" = c."Customer_ID"
GROUP BY s."Customer_ID", c."Customer_Name", c."Segment"
ORDER BY lifetime_value DESC;
```

---

## 🐍 Python Pipeline

```python
# ETL: Load cleaned CSVs into PostgreSQL via SQLAlchemy
from sqlalchemy import create_engine
import pandas as pd

engine = create_engine('postgresql://user:password@localhost:5432/walmart_sales_db')

for table, file in {
    'clean_customer': 'clean_customer.csv',
    'clean_product':  'clean_product.csv',
    'clean_store':    'clean_store.csv',
    'clean_employee': 'clean_employee.csv',
    'clean_sales':    'clean_sales.csv'
}.items():
    pd.read_csv(file).to_sql(table, engine, if_exists='replace', index=False)

print("ETL complete — all 5 tables loaded.")
```

---

## 📈 Dataset

| Attribute | Detail |
|---|---|
| Source | Synthetic dataset modeled on Walmart retail structure |
| Records | 550,000 sales transactions |
| Time Period | January 2020 – December 2023 |
| Tables | 5 (Sales, Customer, Product, Store, Employee) |
| Customers | 1,000 unique customers |
| Products | Multiple categories across 5 departments |

> ⚠️ **Note:** This is a synthetic dataset created for analytical demonstration. It is designed to reflect realistic retail relationships and business patterns, not actual Walmart data.

---

## 🚀 How to Run This Project

**1. Clone the repo**
```bash
git clone https://github.com/seema-kri/walmart-sales-analytics.git
cd walmart-sales-analytics
```

**2. Set up PostgreSQL**
```bash
# Create database
createdb walmart_sales_db
```

**3. Run Python ETL**
```bash
jupyter notebook notebooks/Connection_ETL.ipynb
# Update connection string with your credentials
```

**4. Run SQL Analysis**
```bash
psql -d walmart_sales_db -f sql/sql_business_analysis.sql
```

**5. Open Power BI Dashboard**
```
Open dashboard.pbix in Power BI Desktop
Update data source to your PostgreSQL connection
```

---

## 👩‍💻 About

Built as an end-to-end data analytics portfolio project demonstrating the full analyst workflow:
**raw data → cleaning → storage → SQL analysis → visualization → business insights**

📧 Connect on [LinkedIn](https://www.linkedin.com/in/seema-kumari-375763308/) 
