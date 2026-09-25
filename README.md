

<div align="center">

# 🛍️ Customer Shopping Behavior & Revenue Intelligence
### End-to-End Analytics Pipeline: Python, PostgreSQL Relational Modeling & Power BI Executive BI

[![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15+-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Power BI](https://img.shields.io/badge/Power_BI-Business_Intelligence-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](LICENSE)

<p align="center">
  <b>Uncovering behavioral unit economics, subscription lift, and customer retention levers across 3,900+ multi-category consumer transactions.</b>
</p>

[Analytical Architecture](#-end-to-end-data-pipeline) • [SQL Production Queries](#-core-postgresql-analytics-engine) • [Key Findings](#-strategic-business-insights) • [Dashboard Overview](#-executive-power-bi-intelligence-suite) • [Quickstart](#-reproduce-locally)

</div>

---

## 📌 Executive Summary

Modern retail growth depends on understanding customer lifetime value and discounting sensitivity. When product pricing and promotions operate without transaction analytics, retailers risk discounting high-demand products, misjudging subscription lift, and ignoring friction points in the conversion funnel.

This project delivers an end-to-end retail intelligence solution across **3,900 consumer purchase events[cite: 1]**. Using an integrated stack (**Python ➔ PostgreSQL ➔ Power BI**), the pipeline:
1. Cleans and imputes missing signals with statistical guardrails.
2. Ingests structured data into **PostgreSQL** to execute complex window functions, CTEs, and segmentation logic[cite: 1].
3. Yields executive KPI tracking within an interactive **Power BI dashboard[cite: 1]**, proving that **subscribers spend 32% more than non-subscribers[cite: 1]** and that customer retention drives margin expansion.

---

## 🏗️ End-to-End Data Pipeline

```text
┌─────────────────┐       ┌──────────────────────┐       ┌────────────────────────┐       ┌──────────────────────┐
│  Raw Ingestion  │ ────> │ Python Wrangling/EDA │ ────> │  PostgreSQL Analytics  │ ────> │ Executive Power BI   │
│  3,900 Records  │       │ Pandas / Missing Val │       │ Window Fns / CTEs / RFM│       │ Dashboards & Reports │
└─────────────────┘       └──────────────────────┘       └────────────────────────┘       └──────────────────────┘

```

1. **Ingestion & Data Validation:** Processed 3,900 transaction records containing 18 demographic, operational, and behavioral dimensions.


2. **Deterministic Data Cleaning (Pandas):** Handled missing data in `Review Rating` via category-median imputation to avoid distribution skew; unified naming conventions to `snake_case`; eliminated collinear attributes (`promo_code_used`); derived analytical dimensions (`age_group`, `purchase_frequency_days`).
3. **Database Staging:** Streamlined DataFrame writes via SQLAlchemy/psycopg2 into normalized PostgreSQL staging tables.
4. **Relational Analysis:** Queried 10+ business hypotheses evaluating margin elasticity, cohort trends, and shipping efficiencies.


5. **Business Intelligence Delivery:** Designed dynamic visualizations showcasing customer tiers, frequency trends, and margin-driving segments.



---

## 💡 Strategic Business Insights

| Operational Vector | Empirical Finding | Commercial Strategic Action |
| --- | --- | --- |
| **Subscription Lift** | **Subscribers generate 32% higher average revenue** than non-subscribers.

 | Allocate 25% of acquisition budgets to targeted sign-up incentives and free-trial loyalty funnels. |
| **Shipping Velocity** | Customers selecting **Express Shipping** average substantially higher cart values. | Offer dynamic free express shipping tiers above threshold cart amounts to increase Average Order Value (AOV). |
| **Discount Margin Drag** | Specific recurring items exhibit high sales solely when discounted, cannibalizing raw margins. | Re-evaluate product elasticity; replace blanket discounts with tiered volume thresholds (e.g., Buy 2, Get 10%). |
| **Core Demographic Engine** | Prime purchase concentration centers around defined mid-age cohorts with elevated repeat indices. | Direct performance marketing spend toward this high-converting age demographic. |

---

## 💻 Core PostgreSQL Analytics Engine

Below is a curated sample of analytical queries answering critical business hypotheses:

### 1. Revenue & Average Spend Lift: Subscribers vs. Non-Subscribers

```sql
-- Evaluates transaction volume, total capital contribution, and AOV by subscription tier
SELECT 
    subscription_status,
    COUNT(customer_id) AS total_orders,
    ROUND(SUM(purchase_amount)::NUMERIC, 2) AS total_revenue,
    ROUND(AVG(purchase_amount)::NUMERIC, 2) AS average_order_value,
    ROUND(
        (AVG(purchase_amount) - LAG(AVG(purchase_amount)) OVER (ORDER BY subscription_status DESC)) 
        / LAG(AVG(purchase_amount)) OVER (ORDER BY subscription_status DESC) * 100, 2
    ) AS pct_aov_lift
FROM customer_shopping_data
GROUP BY subscription_status;

```

### 2. Behavioral Segmentation (New, Returning, Loyal Cohorts)

```sql
-- Segments customer base based on historical frequency to evaluate retention depth
WITH customer_cohorts AS (
    SELECT 
        customer_id,
        previous_purchases,
        purchase_amount,
        CASE 
            WHEN previous_purchases = 0 THEN 'New Customer'
            WHEN previous_purchases BETWEEN 1 AND 5 THEN 'Returning Customer'
            ELSE 'Loyal Customer'
        END AS customer_segment
    FROM customer_shopping_data
)
SELECT 
    customer_segment,
    COUNT(customer_id) AS total_customers,
    ROUND(SUM(purchase_amount)::NUMERIC, 2) AS gross_revenue,
    ROUND(AVG(purchase_amount)::NUMERIC, 2) AS cohort_aov
FROM customer_cohorts
GROUP BY customer_segment
ORDER BY gross_revenue DESC;

```

### 3. Top 3 Highest Revenue Products Per Category (Window Dense Rank)

```sql
-- Identifies category anchor products via relational window rankings
WITH ranked_products AS (
    SELECT 
        category,
        item_purchased,
        SUM(purchase_amount) AS total_product_revenue,
        DENSE_RANK() OVER (
            PARTITION BY category 
            ORDER BY SUM(purchase_amount) DESC
        ) AS category_rank
    FROM customer_shopping_data
    GROUP BY category, item_purchased
)
SELECT 
    category,
    category_rank,
    item_purchased,
    total_product_revenue
FROM ranked_products
WHERE category_rank <= 3;

```

---

## 📊 Executive Power BI Intelligence Suite

The report tracks key customer metrics across three primary focal areas:

* **Revenue Performance Matrix:** Dynamic cross-filtering across categories, demographics, and geographical markets.
* **Cohort & Conversion Funnel:** Visual breakdown comparing subscriber vs. non-subscriber behavior and discount reliance.
* **Logistics & Shipping Optimization:** Order size tracking correlated with shipping speeds and review satisfaction ratings.

```text
[ Power BI Report Canvas ]
 ├── Top KPI Cards    : Total Revenue | Gross Orders | Subscription AOV Lift | Median Rating
 ├── Funnel Charts    : Customer Cohort Distribution (New vs. Returning vs. Loyal)
 ├── Matrix Visual    : Category Margin Elasticity vs. Discount Application
 └── Decomposition    : Revenue Contribution Split by Age Bracket and Shipping Method

```

---

## 🛠️ Technology Stack & Dependencies

| Layer | Tools | Focus |
| --- | --- | --- |
| **Language & Environment** | Python 3.9+, Jupyter Notebook, VS Code

 | Modular analysis, exploratory data inspection, and scripting

 |
| **Data Manipulation** | Pandas, NumPy

 | Imputation, feature engineering, and statistical cleaning

 |
| **Database Engine** | PostgreSQL 15, pgAdmin

 | Relational warehousing, window functions, and analytics queries

 |
| **Business Intelligence** | Microsoft Power BI, DAX

 | Executive dashboards, dimensional relationships, and visual metrics

 |
| **Version Control** | Git, GitHub

 | Commit histories, collaborative documentation, and code auditing

 |

---

## 📁 Repository Structure

```bash
├── data/
│   ├── raw/
│   │   └── customer_shopping_data.csv        # Raw dataset (3,900 rows, 18 features)
│   └── processed/
│       └── cleaned_customer_data.csv         # Cleaned, standardized, feature-engineered data
├── sql/
│   ├── 01_schema_setup.sql                   # Database definition & table staging scripts
│   ├── 02_analytical_queries.sql             # Advanced CTEs, segmentation, and window queries
│   └── 03_cohort_analysis.sql                # Retention and subscription behavior queries
├── notebooks/
│   └── customer_behavior_eda_cleaning.ipynb  # End-to-end Python EDA, cleaning & imputation
├── reports/
│   ├── shopping_behavior_dashboard.pbix      # Interactive Power BI report file
│   └── executive_presentation.pdf            # Slide summary of findings & business recommendations
├── requirements.txt                          # Python dependencies
├── LICENSE                                   # MIT License
└── README.md                                 # Technical documentation

```

---

## ⚙️ Reproduce Locally

### 1. Environment Setup

```bash
# Clone repository
git clone [https://github.com/iza684/Customer-Shopping-Behavior-Analysis.git](https://github.com/iza684/Customer-Shopping-Behavior-Analysis.git)
cd Customer-Shopping-Behavior-Analysis

# Initialize virtual environment
python -m venv venv
source venv/bin/activate  # On Windows use: venv\Scripts\activate

# Install required dependencies
pip install -r requirements.txt

```

### 2. Database Initialization

```bash
# Connect to PostgreSQL and create database
psql -U postgres -c "CREATE DATABASE shopping_db;"

# Run schema and ingestion scripts
psql -U postgres -d shopping_db -f sql/01_schema_setup.sql

```

### 3. Run Cleaning & Pipeline

```bash
jupyter notebook notebooks/customer_behavior_eda_cleaning.ipynb

```

---

## 👤 Author & Connect

**Izaan Ansari**

*Data Analyst | Business Intelligence & Machine Learning*

```

```
