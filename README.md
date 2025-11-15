
# Customer Shopping Behavior Analysis – End-to-End Data Analytics Project
Overview

This project analyzes customer shopping behavior using transactional data from 3,900 purchases across multiple product categories.
The goal is to identify spending patterns, customer segments, subscription influence, product performance, and behavior trends to support data-driven business decisions.

The project follows a complete analytics workflow:
Python → EDA → Cleaning → PostgreSQL SQL Analysis → Power BI Dashboard → Final Recommendations.

Dataset

Total Rows: 3,900
Total Columns: 18

Key Features

Customer Information: Age, Gender, Location, Subscription Status

Purchase Details: Item Purchased, Category, Purchase Amount, Season, Size, Color

Behavioral Attributes: Discount Applied, Promo Code Used, Previous Purchases, Frequency of Purchases, Review Rating

Logistics: Shipping Type

Missing Data

37 missing values in Review Rating (handled during cleaning)

Tools & Technologies

Python: Pandas, NumPy

PostgreSQL + pgAdmin: SQL analysis

Power BI: Dashboard creation

Gamma: Final presentation

Jupyter Notebook / VS Code

GitHub for version control

Project Workflow
1. Data Loading (Python)

Imported CSV using Pandas

Inspected structure using info() and describe()

Checked missing values and data distributions

2. Exploratory Data Analysis (EDA)

Summary statistics

Visual distribution of age, spending, categories

Seasonal trends and product behavior

Checked correlations and customer patterns

3. Data Cleaning

Imputed missing Review Ratings using the median per category

Standardized column names to snake_case

Dropped redundant column (promo_code_used)

Created new features:

age_group (binned categories)

purchase_frequency_days

Ensured consistency between discount_applied and price fields

4. Loading into PostgreSQL

Established DB connection from Python

Loaded cleaned DataFrame into PostgreSQL for SQL-based analysis

5. SQL Analysis (Key Business Questions)

Performed structured analysis using SQL to answer:

Revenue comparison by Gender

High-spending customers who still use discounts

Top 5 products by average Review Rating

Standard vs Express Shipping Type performance

Subscriber vs Non-Subscriber revenue and average spend

Products most dependent on discounts

Customer segmentation into New, Returning, Loyal

Top 3 products per category

Whether repeat buyers (>5 purchases) are more likely to subscribe

Revenue contribution across Age Groups

6. Power BI Dashboard

Built an interactive dashboard showing:

Revenue KPIs

Category-wise and gender-wise sales

Age group insights

Discount trends

Subscriber vs non-subscriber metrics

Shipping type comparisons

7. Final Report (Gamma)

Summarized:

Workflow

Insights

Dashboard visuals

Business recommendations

Key Insights

Subscription customers contribute significantly higher revenue.

Loyal customers (high repeat purchases) show strong retention potential.

Certain products rely heavily on discounts; review pricing strategy.

Express shipping customers spend more on average.

Top-rated products and categories can be prioritized for marketing.

Highest revenue contribution comes from strong-performing age groups.

Business Recommendations

Improve subscription benefits to boost conversions.

Launch loyalty and rewards programs.

Optimize discount strategy to balance margin and sales volume.

Feature top-rated products more prominently.

Target high-value customer segments and high-spending age groups.
