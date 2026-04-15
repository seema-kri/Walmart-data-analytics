# 🛒 Walmart BI Analytics Platform

🚀 Built a production-ready BI system analyzing **$764M in retail data** to drive executive decision-making.

---

## 🚨 Why This Project Stands Out

Most portfolios show *analysis*. This project demonstrates **business impact, scalability, and decision-making clarity**.

✔ Built like a real-world BI system used by enterprise teams
✔ Processes **550,000+ transactions** across multiple dimensions
✔ Answers **critical business questions executives actually ask**
✔ Delivers a **decision-ready dashboard — not just charts**

---

## 📊 Dashboard Preview

### Executive Summary

![Executive](images/Excecutive%20Summary.png)

### Product & Store Insights

![Product](images/Product%20%26%20Store.png)

### Customer & Employee Insights

![Customer](images/Customer%20%26%20Employee.png)

> 3-page interactive Power BI dashboard designed for leadership-level decision making.

---

## 💼 Business Problem

Retail leaders lack a **single source of truth** to answer:

* Why has revenue plateaued?
* Which categories are growing vs declining?
* Where are we losing market share?
* Who are our most valuable customers and employees?

This project solves that.

---

## 🧠 What I Built

A complete **end-to-end BI pipeline** — from raw data to executive insights:

### 🔹 Data Engineering (Python · pandas)

* Cleaned **550K+ rows** with real-world inconsistencies
* Removed duplicates, handled nulls, standardized formats
* Ensured **referential integrity across 5 tables**

### 🔹 Data Warehouse (PostgreSQL)

* Designed a **star schema (1 fact + 4 dimensions)**
* Enabled scalable analytics using SQL joins & indexing

### 🔹 Advanced Analytics (SQL + Python)

* Built **15 business-critical queries**
* Used **window functions (LAG, RANK, DENSE_RANK)**
* Performed trend, cohort, and performance analysis

### 🔹 Executive Dashboard (Power BI)

* 3 interactive pages
* 20+ DAX measures
* Time intelligence (YoY, MoM, Rolling 12M)
* Drill-down capability for decision-makers

---

## 📈 Key Business Insights

### 🚩 Revenue Stagnation

* Monthly revenue stuck between **$14M–$17M for 4 years**
  👉 Indicates **market saturation → growth strategy required**

### 🌍 Regional Opportunity

* Central: **$221M** vs West: **$162M**
  👉 **$59M untapped potential**

### 📚 Category Performance

* Books: **+3.6% YoY (only growing category)**
* Clothing: **declining (-1.3%)**
  👉 Reallocate investment

### 👥 Customer Imbalance

* Top 10 customers = **100% Consumer segment**
  👉 B2B underdeveloped (**$367M gap**)

### 🧑‍💼 Employee Gap

* Top performer = **2.25× average**
* ~35% below average
  👉 Training + performance optimization needed

---

## 🧱 Tech Stack

| Layer           | Tools                        |
| --------------- | ---------------------------- |
| Data Processing | Python, pandas               |
| Database        | PostgreSQL                   |
| Data Access     | SQLAlchemy, psycopg2         |
| Analysis        | SQL (CTEs, Window Functions) |
| Visualization   | Power BI                     |

---

## 🗂️ Project Structure

```
walmart-bi-analytics-platform/
│
├── notebooks/              # Data cleaning, ETL, EDA
├── sql/                    # Business analysis queries
├── data/                   # Raw datasets
├── images/                 # Dashboard previews
├── dashboard.pbix          # Power BI file
└── README.md
```

---

## ⚡ What Makes This Hiring-Ready

This is not a tutorial project.

This demonstrates:

* ✅ Business thinking (not just coding)
* ✅ Production-level SQL skills
* ✅ Data storytelling for executives
* ✅ End-to-end ownership (data → insight → decision)

---

## 🚀 How to Run

```bash
# Clone repo
git clone https://github.com/yourusername/walmart-bi-analytics-platform
cd walmart-bi-analytics-platform

# Install dependencies
pip install pandas sqlalchemy psycopg2 matplotlib seaborn

# Create PostgreSQL database
psql -U postgres -c "CREATE DATABASE walmart_sales_db;"

# Run notebooks
jupyter notebook Data_Cleaning.ipynb
jupyter notebook Connection_ETL.ipynb
jupyter notebook EDA.ipynb
```

---

## 👤 About Me

I build data systems that answer **real business questions — not just dashboards**.

🔗 LinkedIn: https://www.linkedin.com/in/seema-kumari-375763308/
🔗 GitHub: https://github.com/seema-kri

---

## ⭐ Final Note

If you're hiring for:

* Data Analyst
* BI Analyst
* Analytics Engineer

This project reflects how I would **deliver impact from Day 1**.
