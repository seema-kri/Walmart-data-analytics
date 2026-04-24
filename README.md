# Walmart Sales Analytics — End-to-End BI Platform

> Walmart's leadership had no single view of which regions, categories, and customer segments were driving — or dragging — revenue. This project builds that system: a production-grade BI pipeline that turns 550K+ raw transactions into boardroom-ready decisions.

[![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python)](https://python.org)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Star%20Schema-336791?logo=postgresql)](https://postgresql.org)
[![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi)](https://powerbi.microsoft.com)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

---

## Dashboard Preview

| Executive Summary | Product & Store | Customer & Employee |
|---|---|---|
| ![Executive Summary](images/Excecutive%20Summary.png) | ![Product & Store](images/Product%20%26%20Store.png) | ![Customer & Employee](images/Customer%20%26%20Employee.png) |

---

## Table of Contents

- [Problem Statement](#problem-statement)
- [Pipeline Architecture](#pipeline-architecture)
- [What I Built](#what-i-built)
- [Key Business Insights](#key-business-insights)
- [Project Structure](#project-structure)
- [Tech Stack](#tech-stack)
- [Run It Yourself](#run-it-yourself)
- [Future Work](#future-work)
- [About](#about)

---

## Problem Statement

Retail leaders need answers to three questions every quarter: *Where is revenue leaking? Which segments are underserved? Who on the team is underperforming?* Raw transaction exports can't answer these — they need a system.

This project engineers that system end-to-end: messy CSVs go in, executive decisions come out. No BI tool vendor, no pre-built connectors — built from scratch using Python, PostgreSQL, SQL, and Power BI.

**Scope:** 550K+ transactions · $764M revenue · 4 years of data · 5 product categories · 4 regions · 93 employees

---

## Pipeline Architecture

```
Raw CSV → Python (pandas) → PostgreSQL star schema → SQL analytics → Power BI dashboard
```

```
┌─────────────┐    ┌─────────────┐    ┌──────────────────┐    ┌──────────────┐    ┌─────────────┐
│   Raw CSV   │───▶│   pandas    │───▶│   PostgreSQL     │───▶│     SQL      │───▶│   Power BI  │
│  5 tables   │    │  Cleaning   │    │   Star Schema    │    │  15 queries  │    │  3-page     │
│  550K rows  │    │  + ETL      │    │  1 fact + 4 dim  │    │  analytics   │    │  dashboard  │
└─────────────┘    └─────────────┘    └──────────────────┘    └──────────────┘    └─────────────┘
```

---

## What I Built

### 1. Data Engineering
Cleaned 550K+ rows across 5 raw tables: removed duplicates, resolved nulls, standardized date formats, and enforced referential integrity. No analysis ran on dirty data.

### 2. Data Warehouse
Designed a **star schema** in PostgreSQL — 1 fact table (`fact_sales`) joined to 4 dimension tables (`dim_product`, `dim_customer`, `dim_employee`, `dim_date`). Enables scalable analytical joins without query rewrites as volume grows.

### 3. SQL Analytics
Wrote **15 business-critical queries** using:
- CTEs for readable, multi-step logic
- Window functions: `LAG`, `RANK`, `DENSE_RANK` for time-series and ranking analysis
- Time-series aggregations for YoY, MoM, and rolling comparisons

Each query answers a specific executive question — not a generic data dump.

### 4. Executive Dashboard
Built a **3-page Power BI dashboard** with 20+ DAX measures including time intelligence (YoY growth, MoM change, Rolling 12M). Designed for leadership decision-making, not data exploration.

| Page | Focus |
|---|---|
| Executive Summary | Revenue trends, regional performance, KPIs |
| Product & Store | Category analysis, store-level breakdown |
| Customer & Employee | Segment LTV, employee contribution ranking |

---

## Key Business Insights

### Revenue Stagnation
Monthly revenue has been flat at **$14M–$17M for 4 consecutive years** despite 550K+ transactions. Incremental optimization won't fix this — the data points to market saturation requiring a strategic pivot.

### $59M Regional Gap
The Central region generates **$221M vs West's $162M** — a $59M gap that holds across every product category, with Central outperforming by $12–13M per category. This is the largest single addressable growth lever in the data.

### Books Is the Only Growing Category
Books grew **+3.6% YoY in 2023** (Fiction +8.5%, Non-Fiction +5.2%). Clothing dropped –1.3%. Budget reallocation from Clothing to Books is directly supported by the data.

### B2B Channel Severely Underdeveloped
Consumer segment: **$519M**. Corporate: **$153M**. Zero corporate customers appear in the top 10 LTV list. The $367M gap represents an untapped revenue channel with no current organizational focus.

### Employee Performance Spread
The top performer generates **2.25× the average revenue contribution**. Only 33 of 93 employees exceed average — a structured coaching or incentive program has measurable, quantifiable ROI.

---

## Project Structure

```
walmart-bi-analytics-platform/
│
├── notebooks/
│   ├── Data_Cleaning.ipynb         # 550K+ row cleaning pipeline
│   ├── Connection_ETL.ipynb        # PostgreSQL load via SQLAlchemy
│   └── EDA.ipynb                   # Exploratory analysis
│
├── sql/
│   ├── 01_revenue_trends.sql       # Monthly/YoY revenue analysis
│   ├── 02_regional_performance.sql # Region vs region breakdown
│   ├── 03_category_analysis.sql    # Product category trends
│   ├── 04_customer_ltv.sql         # Customer lifetime value ranking
│   └── 05_employee_performance.sql # Employee contribution analysis
│
├── data/
│   └── schema.sql                  # PostgreSQL star schema DDL
│
├── images/                         # Dashboard screenshots
├── dashboard.pbix                  # Power BI file (open in Power BI Desktop)
├── requirements.txt                # Python dependencies
└── README.md
```

---

## Tech Stack

| Layer | Tools |
|---|---|
| Data processing | Python 3.11, pandas |
| Database | PostgreSQL 15 |
| Data access | SQLAlchemy, psycopg2 |
| Analytics | SQL — CTEs, window functions, time-series aggregations |
| Visualization | Power BI Desktop, DAX (20+ measures, time intelligence) |

---

## Run It Yourself

### Prerequisites
- Python 3.9+
- PostgreSQL 13+
- Power BI Desktop (Windows)

### Setup

```bash
# 1. Clone the repo
git clone https://github.com/seema-kri/walmart-bi-analytics-platform
cd walmart-bi-analytics-platform

# 2. Install dependencies
pip install -r requirements.txt

# 3. Create PostgreSQL database and schema
psql -U postgres -c "CREATE DATABASE walmart_sales_db;"
psql -U postgres -d walmart_sales_db -f data/schema.sql

# 4. Run notebooks in order
jupyter notebook notebooks/Data_Cleaning.ipynb
jupyter notebook notebooks/Connection_ETL.ipynb
jupyter notebook notebooks/EDA.ipynb
```

Then open `dashboard.pbix` in Power BI Desktop and refresh the data source connection.

---

## Future Work

- **Live data pipeline** — connect to a streaming POS API to replace batch CSV ingestion
- **Automated anomaly alerts** — trigger Slack/email notifications when weekly revenue drops >10% MoM
- **Corporate customer model** — build a propensity model to identify consumer customers most likely to convert to B2B
- **Dashboard embedding** — publish to Power BI Service and embed in a web app for demo access

---

## About

I build data systems that surface decisions — not just charts.

**[LinkedIn](https://www.linkedin.com/in/seema-kumari-375763308/)** · **[GitHub](https://github.com/seema-kri)**
