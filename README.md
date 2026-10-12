# 🛍️ Customer Shopping Behavior & Revenue Analysis
![Python](https://img.shields.io/badge/Language-Python-blue)
![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL-blue)
![Power BI](https://img.shields.io/badge/Tool-Power%20BI-yellow)
![Analysis](https://img.shields.io/badge/Type-Customer%20Analytics-orange)
![Status](https://img.shields.io/badge/Status-Complete-success)

## 🎯 Project Overview
Modern retail growth depends on understanding customer lifetime value and how customers react to discounts. Without transaction analytics, retailers risk discounting products that already sell well, misjudging the value of subscriptions, and missing friction points in the conversion funnel.

This project analyzes **3,900 consumer purchase transactions** using Python, PostgreSQL, and Power BI. It cleans and imputes missing data, uses SQL (window functions, CTEs, and segmentation) to answer business questions, and presents the results in an interactive dashboard. The analysis shows that **subscribers spend 32% more than non-subscribers** and that customer retention drives margin expansion.

---

## 📈 Key Metrics
| Metric | Value |
|--------|-------|
| **Total Transactions** | 3,900 |
| **Features per Transaction** | 18 (demographic, operational, and behavioral) |
| **Business Questions Analyzed** | 10+ |
| **Subscriber Spend Lift** | 32% higher than non-subscribers |
| **Tools** | Python, PostgreSQL, Power BI |

---

## 🔍 Key Findings

### 💡 Subscription Lift
> **Subscribers generate 32% higher average revenue than non-subscribers**

Subscribers are clearly more valuable per customer, which makes subscription sign-ups a strong growth lever.

### 🚚 Shipping Speed and Cart Value
> **Customers who choose Express Shipping have substantially higher average cart values**

Faster shipping is linked with larger purchases.

### 🏷️ Discount Margin Drag
> **Some recurring items sell well only when discounted**

These discounts reduce raw margins and cannibalize profit.

### 👥 Core Customer Group
> **Purchases are concentrated among mid-age customers with higher repeat purchase rates**

This age group is the main engine of repeat business.

---

## 💼 Business Recommendations
Based on the analysis, the following actions are recommended:

1. **Invest in Subscriptions**
   Allocate 25% of acquisition budgets to sign-up incentives and free-trial loyalty funnels

2. **Use Free Express Shipping Tiers**
   Offer free express shipping above a minimum cart amount to increase Average Order Value (AOV)

3. **Rethink Blanket Discounts**
   Re-evaluate product price elasticity and replace blanket discounts with tiered volume offers (for example, Buy 2, Get 10%)

4. **Target the Core Age Group**
   Direct performance marketing spend toward the high-converting mid-age demographic

---

## 🛠️ Technical Approach

### Data Pipeline
```text
┌─────────────────┐       ┌──────────────────────┐       ┌────────────────────────┐       ┌──────────────────────┐
│  Raw Ingestion  │ ────> │ Python Wrangling/EDA │ ────> │  PostgreSQL Analytics  │ ────> │ Executive Power BI   │
│  3,900 Records  │       │ Pandas / Missing Val │       │ Window Fns / CTEs / RFM│       │ Dashboards & Reports │
└─────────────────┘       └──────────────────────┘       └────────────────────────┘       └──────────────────────┘
```

### Python & Pandas:
- **Ingestion & Validation**: Processed 3,900 transaction records with 18 demographic, operational, and behavioral columns
- **Data Cleaning**: Filled missing `Review Rating` values with the category median to avoid skewing the distribution
- **Standardization**: Unified column names to `snake_case` and removed the collinear column `promo_code_used`
- **Feature Engineering**: Created new columns `age_group` and `purchase_frequency_days`

### PostgreSQL:
- **Database Staging**: Wrote the cleaned DataFrame into normalized PostgreSQL staging tables using SQLAlchemy and psycopg2
- **Relational Analysis**: Queried 10+ business questions covering margin elasticity, cohort trends, and shipping efficiency using window functions, CTEs, and segmentation

### Power BI:
- **Dashboard Design**: Built interactive visuals showing customer tiers, frequency trends, and margin-driving segments

---

## 💻 SQL Queries

A selection of queries that answer key business questions:

### 1. Revenue and Average Spend Lift: Subscribers vs. Non-Subscribers

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

### 2. Behavioral Segmentation (New, Returning, Loyal Customers)

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

## 📊 Power BI Dashboard

The report tracks key customer metrics across three areas:

- **Revenue Performance:** Cross-filtering across categories, demographics, and geographical markets
- **Cohort & Conversion Funnel:** Comparison of subscriber and non-subscriber behavior and discount reliance
- **Logistics & Shipping:** Order size tracking linked to shipping speed and review ratings

```text
[ Power BI Report Canvas ]
 ├── Top KPI Cards    : Total Revenue | Gross Orders | Subscription AOV Lift | Median Rating
 ├── Funnel Charts    : Customer Cohort Distribution (New vs. Returning vs. Loyal)
 ├── Matrix Visual    : Category Margin Elasticity vs. Discount Application
 └── Decomposition    : Revenue Contribution Split by Age Bracket and Shipping Method
```

---

## 🧰 Technology Stack

| Layer | Tools | Focus |
|-------|-------|-------|
| **Language & Environment** | Python 3.9+, Jupyter Notebook, VS Code | Modular analysis, exploratory data inspection, and scripting |
| **Data Manipulation** | Pandas, NumPy | Imputation, feature engineering, and statistical cleaning |
| **Database Engine** | PostgreSQL 15, pgAdmin | Relational warehousing, window functions, and analytics queries |
| **Business Intelligence** | Microsoft Power BI, DAX | Executive dashboards, dimensional relationships, and visual metrics |
| **Version Control** | Git, GitHub | Commit histories, collaborative documentation, and code auditing |

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
└── README.md                                 # Project documentation
```

---

## ⚙️ Reproduce Locally

### 1. Environment Setup

```bash
# Clone repository
git clone https://github.com/iza684/Customer-Shopping-Behavior-Analysis.git
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

## 📧 Author

**Izaan Ansari**
*Data Analyst | Business Intelligence & Machine Learning*

---

⭐ **If you found this project useful, please consider giving it a star!**
