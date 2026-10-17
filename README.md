# 🛍️ Customer Shopping Behavior & Revenue Analysis
![Python](https://img.shields.io/badge/Language-Python-blue)
![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL-blue)
![Power BI](https://img.shields.io/badge/Tool-Power%20BI-yellow)
![Analysis](https://img.shields.io/badge/Type-Customer%20Analytics-orange)
![Status](https://img.shields.io/badge/Status-Complete-success)

## 🎯 Project Overview
Modern retail growth depends on understanding customer lifetime value and how customers react to discounts. Without transaction analytics, retailers risk discounting products that already sell well, misjudging the value of subscriptions, and missing friction points in the conversion funnel.

This project analyzes **3,900 consumer purchase transactions** using Python, PostgreSQL, and Power BI. It cleans and imputes missing data, uses SQL (window functions, CTEs, and segmentation) to answer business questions, and presents the results in a **3-page interactive Power BI report** (Overview, Behavior, Insights). The analysis shows that **subscribers spend 32% more than non-subscribers** and that customer retention drives margin expansion.

---

## 🖼️ Dashboard Preview

### Before vs After
I rebuilt my first dashboard from scratch: from basic charts showing totals to a 3-page report with navigation, a consistent design system, and actionable insights.

| Before (v1) | After (v2) |
|:---:|:---:|
| <img width="987" height="540" alt="Image" src="https://github.com/user-attachments/assets/d580c1ff-ef1e-41b1-9674-8b87f19aa5c6" />| <img width="1064" height="596" alt="Image" src="https://github.com/user-attachments/assets/2714df22-2333-4d50-9883-26f7b0943162" />|

### Page 1: Overview
Revenue by category, age group, season and location, with subscription split and a Top/Bottom N location toggle.

<img width="1064" height="596" alt="Image" src="https://github.com/user-attachments/assets/2714df22-2333-4d50-9883-26f7b0943162" />
### Page 2: Behavior Deep-Dive
Discount usage, repeat customers, revenue decomposition by gender, age group and category, a season heatmap, and subscriber rate by purchase frequency.
<img width="1065" height="599" alt="Image" src="https://github.com/user-attachments/assets/3f91f588-1603-4c76-a180-c64e8adbf1d5" />
### Page 3: Insights
Key findings with evidence and recommendations.

<img width="1058" height="597" alt="Image" src="https://github.com/user-attachments/assets/56e4c7ec-70b7-4bbd-ab75-0e867eac6f77" />

---

## 📈 Key Metrics
| Metric | Value |
|--------|-------|
| **Total Transactions** | 3,900 |
| **Total Revenue** | $233K |
| **Average Purchase Amount** | $59.76 |
| **Average Review Rating** | 3.75 |
| **Subscribers** | 27% of customers |
| **Discount Usage** | 43% of customers |
| **Repeat Customers** | 49.62% |
| **Features per Transaction** | 18 (demographic, operational, and behavioral) |
| **Business Questions Analyzed** | 10+ |
| **Subscriber Spend Lift** | 32% higher than non-subscribers |
| **Tools** | Python, PostgreSQL, Power BI |

---

## 🔍 Key Findings

### 📦 Category Drives Revenue
> **Clothing (44.7%) and Accessories (31.8%) generate ~76% of total revenue**

Outerwear contributes only ~8%. Revenue across top states (5.5K to 5.8K) and seasons (56K to 60K) is almost flat, so product category matters far more than geography.

### 👥 Male-Heavy Base, Low Subscription
> **68% of customers are male and only 27% are subscribers**

Female customers and the 73% non-subscribers are the biggest untapped growth segments.

### 🎯 Young Adults Lead, Peaking in Winter
> **Young Adults generate the highest revenue (62K) and peak in Winter (16.1K); Seniors dip to their lowest in Winter (12.9K)**

The lead over other age groups is modest (about 10%), so it is a slight edge rather than a dominant trend.

### 🔁 Subscription Rate and Purchase Frequency
> **Weekly buyers have the highest subscriber rate (29.1%) vs Quarterly buyers (24.9%)**

A gap of about 4 points: frequent buyers are slightly more open to subscribing.

### 💡 Subscription Lift
> **Subscribers generate 32% higher average revenue than non-subscribers**

Subscribers are clearly more valuable per customer, which makes subscription sign-ups a strong growth lever.

### 🚚 Shipping Speed and Cart Value
> **Customers who choose Express Shipping have substantially higher average cart values**

Faster shipping is linked with larger purchases.

### 🏷️ Discount Margin Drag
> **Some recurring items sell well only when discounted**

These discounts reduce raw margins and cannibalize profit.

---

## 💼 Business Recommendations
Based on the analysis, the following actions are recommended:

1. **Focus on High-Revenue Categories**
   Prioritize marketing budget on Clothing and Accessories, and push Outerwear through bundles and winter offers

2. **Invest in Subscriptions**
   Allocate 25% of acquisition budgets to sign-up incentives and free-trial loyalty funnels, and offer subscriptions at the point of repeat purchase

3. **Use Free Express Shipping Tiers**
   Offer free express shipping above a minimum cart amount to increase Average Order Value (AOV)

4. **Rethink Blanket Discounts**
   Re-evaluate product price elasticity and replace blanket discounts with tiered volume offers (for example, Buy 2, Get 10%)

5. **Target Under-Served and High-Value Segments**
   Run campaigns to attract female customers, target Winter campaigns at Young Adults, and design a separate seasonal offer for Seniors

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
- **Data Modeling**: Dedicated `_Measures` table with DAX measures for revenue, customer counts, subscriber metrics, discount usage, and repeat-customer rate
- **Dashboard Design**: 3-page report with a consistent theme, page navigation buttons, and slicers
- **Dynamic Formatting**: Conditional bar colors driven by a DAX measure so the top performer is highlighted automatically, even when filters or the Top/Bottom toggle change
- **Advanced Visuals**: Decomposition Tree, conditional-format heatmap matrix, Top N filtering, and dropdown/tile slicers

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

A 3-page interactive report with navigation buttons between pages.

### Page 1: Overview
- **KPI Cards:** Total Customers | Revenue | Avg Purchase Amount | Avg Review Rating
- **Charts:** Location by Revenue (Top/Bottom N toggle), Customers by Subscription Status (donut), Revenue by Age Group, Category and Season
- **Slicers:** Dropdown filters in the header
- **Highlighting:** The top performer in each chart is automatically highlighted

### Page 2: Behavior Deep-Dive
- **KPI Cards:** Discount Usage % | Avg Previous Purchases | Revenue per Customer | Repeat Customer %
- **Decomposition Tree:** Customers split by Gender, Age Group and Category
- **Heatmap Matrix:** Revenue by Age Group and Season
- **Column Chart:** Subscriber % by Purchase Frequency

### Page 3: Insights
- Key findings with supporting evidence and recommendations in a single readable page

```text
[ Power BI Report ]
 ├── Overview   : KPI cards | Top N location bars | Subscription donut | Age / Category / Season charts
 ├── Behavior   : KPI cards | Decomposition Tree | Age x Season heatmap | Subscriber % by frequency
 └── Insights   : Evidence-backed findings and recommendations
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
├── assets/
│   ├── dashboard_v1_before.png               # Original dashboard (before)
│   ├── dashboard_v2_overview.png             # Rebuilt dashboard: Overview page
│   ├── dashboard_v2_behavior.png             # Rebuilt dashboard: Behavior page
│   └── dashboard_v2_insights.png             # Rebuilt dashboard: Insights page
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
│   ├── shopping_behavior_dashboard.pbix      # Interactive Power BI report file (3 pages)
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

### 4. Open the Dashboard
Open `reports/shopping_behavior_dashboard.pbix` in Power BI Desktop.

---

## 📧 Author

**Izaan Ansari**
*Data Analyst | Business Intelligence & Machine Learning*

---

⭐ **If you found this project useful, please consider giving it a star!**
