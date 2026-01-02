# <p align="center"> CAPSTONE PROJECT
</p>

# <p align="center">  📊 Customer Segmentation & Retention Analysis for Skincare & Beauty E-Commerce with Power BI
</p>

<p align="center"> <strong>Author:</strong> Phan Anh Hoang
</p>

## Project Overview

This portfolio project presents a comprehensive **Customer Retention Analysis** and **Customer Segmentation Analysis** using Power BI for a global skincare and beauty e-commerce store. The project leverages advanced analytical techniques including RFM modeling and Cohort analysis to provide actionable insights for improving customer loyalty and optimizing marketing strategies.

👉 [Access Power BI Report PDF Visualization](E-Commerce_C19_FP20analytics_English.pdf)  
👉 [Download Interactive Power BI Report (.pbix)](E-Commerce_C19_FP20analytics_English.pbix)

---
## Table of Contents
  - **[🧰 A. Key Project Information](#-a-key-project-information)**
  - **[🎯 B. Project Goals \& Stakeholder Needs](#-b-project-goals--stakeholder-needs)**
  - **[ ⚙  C. Project Execution](#-c-project-execution)**
  - **[💡 D. Strategic Recommendations](#-d-strategic-recommendations)**
  - **[🚀 Conclusion](#-conclusion)**

---

## 🧰 A. Key Project Information

### 1. Tools & Skills Used

- **Power Query**: Applied in-depth Power Query knowledge for efficient data transformation, cleaning, and preparation with **51,290 transaction records**
- **Data Modeling**: Utilized advanced data modeling techniques including star schema to create optimized data model with fact and dimension tables
- **DAX (Data Analysis Expressions)**: Developed complex measures and calculated columns for:
  - RFM scoring (Recency, Frequency, Monetary)
  - Customer segmentation logic
  - Cohort retention rates
  - Customer Lifetime Value (LTV) calculations
- **Data Visualization**: Designed and implemented appropriate chart types including:
  - Matrix tables for cohort analysis
  - Scatter charts for RFM segment visualization
  - Area charts for trend analysis
- **Report Interactivity**: Integrated essential functionalities such as:
  - Drill-down and drill-through capabilities
  - Tooltip pages for contextual information
  - Slicers for dynamic filtering (Segment, RFM_segment, Year, Market)
  - Bookmarks for navigation between analysis pages
- **Report Design**: Created complete and professional three-page report layout suitable for executive presentation

### 2. Project Introduction

This project is built upon a comprehensive order dataset capturing the complete e-commerce transaction history of a global skincare and beauty retailer.

#### a) About the Company

The company is an anonymous global online retailer specializing in skincare and beauty products. Key characteristics:

- **Geographic Reach**: Serves 164 countries across multiple markets (Western Europe, Central America, Southeast Asia, Oceania, South America)
- **Product Portfolio**: Offers nearly 4,000 products across 5 main categories: Body care, Home and Accessories, Make up, Hair care, and Face care
- **Sales Channel**: Pure-play e-commerce model operating through online platforms
- **Customer Segments**: Serves Consumer, Corporate, and Self-Employed segments

#### b) Company Data Description

The dataset is provided by **FP20 Analytics** and contains historical order data with the following structure:

| Field | Description |
|-------|-------------|
| **Row ID** | Unique identifier for each row |
| **Order ID** | Order identifier (one order can contain multiple items) |
| **Order Date** | Date when order was placed |
| **Customer ID** | Customer identifier (one customer can place multiple orders) |
| **Segment** | Customer segment (Consumer/Corporate/Self-Employed) |
| **City** | Order city |
| **State** | Order state (where applicable) |
| **Country** | Order country |
| **Country latitude/longitude** | Geographic coordinates |
| **Region** | Geographic region (Western Europe, Central America, etc.) |
| **Market** | Market classification |
| **Subcategory** | Product subcategory |
| **Category** | Product category (Body care, Face care, etc.) |
| **Product** | Product name |
| **Quantity** | Number of products purchased per order |
| **Sales** | Total sales amount in USD |
| **Discount** | Discount applied to the order |
| **Profit** | Total profit after discount |

**Data Source**: [FP20 Analytics Dataset](https://fp20analytics.com/wp-content/uploads/2024/08/English-Dataset.zip)

---

## 🎯 B. Project Goals & Stakeholder Needs

### 1. Scenario

As a BI Analyst supporting the Customer Service department, I was tasked with building comprehensive Power BI reports to analyze customer behavior, segmentation, and retention patterns. This analysis would inform strategic decisions on customer acquisition, retention programs, and targeted marketing campaigns.

### 2. Stakeholder Requirements

The Head of Customer Service requested data transformation and analysis within Power BI to provide the following insights:

#### a) Customer Segmentation Analysis (RFM Model)

**Objective**: Classify the customer base using the RFM (Recency-Frequency-Monetary) model to enable targeted marketing and customer care programs.

**Requirements**:
- Calculate individual and aggregate RFM metrics:
  - **Recency**: Days since last purchase
  - **Frequency**: Total number of orders
  - **Monetary**: Total purchase value
- Assign RFM scores (1-5 scale) to each customer
- Classify customers into strategic segments
- Calculate customer counts and characteristics for each segment
- Analyze demographic distribution across segments
- Create detailed customer-level RFM tables with full demographic data

#### b) Customer Retention Analysis (Cohort Analysis)

**Objective**: Understand patterns of new customer acquisition and retention over time to improve retention strategies.

**Requirements**:
- Calculate number of new customers acquired by Month
- Calculate percentage of new customers returning for subsequent purchases by Month
- Create cohort matrix tables displaying:
  - Number of new customers by cohort month
  - Customer retention rates across subsequent months
- Implement comprehensive filtering capabilities using slicers:
  - Segment (Consumer/Corporate/Self-Employed)
  - RFM segment
  - Year
  - Market
  - Region/Country/City hierarchy

#### c) Customer Lifetime Value (LTV) Analysis

**Objective**: Track and predict the cumulative value customers bring throughout their lifecycle.

**Requirements**:
- Calculate LTV by cohort month and RFM segment
- Track LTV progression over 12-month period
- Identify high-value customer segments for investment prioritization

---

## ⚙ C. Project Execution

### 1. Data Collection & Understanding

- Downloaded FP20 Analytics dataset (Excel format)
- Explored data structure, relationships, and data quality
- Identified **51,290 valid transactions** across **17,415 unique customers**
- Analyzed date range: 2020-2023 (primary focus on 2023)

### 2. Data Preparation & Modeling

**Power Query Transformations**:
- Removed null values in critical fields (Customer ID, Sales, Quantity)
- Created calculated columns for:
  - RFM metrics calculation
  - RFM scoring
  - RFM segments
  - Date values for cohort analysis
- Filtered invalid transactions

**Data Model (Power Pivot)**:
- Implemented star schema with:
  - **Fact Table**: fact_sales (grain: one row per product in order)
  - **Dimension Tables**: 
    - dim_customer
    - dim_date (2020-2023)
    - dim_RFMsegment

<p align="center">
  <img src="Images/Model.png" alt="Model" />
</p>

**Key DAX Measures**:

```dax
// Customer Metrics
Total Customers = DISTINCTCOUNT(fact_sales[Customer ID])
Returning Customers = [Total Customers] - [Total New Customers]

// Revenue Metrics
Total Sales = SUM(fact_sales[Sales])
Avg Order Value = DIVIDE(SUM(fact_sales[Sales]), COUNT(fact_sales[Order ID]))
Avg Purchase Frequency = DIVIDE(COUNT(fact_sales[Order ID]), DISTINCTCOUNT(fact_sales[Customer ID]))
```

**Key Calculated Columns**:

```dax
// RFM Calculations
total_recency = DATEDIFF([date_last_purchase], MAX(fact_sales[Order Date]), DAY)
total_frequency = CALCULATE(DISTINCTCOUNT(fact_sales[Order ID]))
total_monetary = CALCULATE(SUM(fact_sales[Sales]))

// RFM Scoring (1-5 scale using PERCENTILE.EXC)
```
**LTV Formula**: This project uses the simplified LTV calculation:  
**LTV = Cumulative Revenue Over Time / Total Number of Customers**

### 3. 📈 Key Insights and Visualizations (Target Customer: Consumer)

Developed interactive three-page report:

#### **Page 1: Customer Overview** (focused on 2023)
<p align="center">
  <img src="Images/Page-1.png" alt="P1" />
</p>

### 📍 Customer Overview Insights (2023)

* **Customer growth is strong but shallow:** Total customers +25.9%, returning customers +73.2%, yet repeat behavior rarely persists beyond the second purchase.
* **Revenue grows by volume, not value:** Revenue +21.5% while AOV –6.9%, showing limited upsell, cross-sell, and basket expansion.
* **Purchase frequency is structurally low:** 1.16 purchases/year (~every 10 months), far below the 1–3 month skincare repurchase cycle.
* **Growth is uneven by region:** SEA & Oceania grow fastest (~2× others), while Western Europe & Central America still account for >50% of customers and must remain stable.

#### **Page 2: Segmentation Analysis (RFM)** 
<p align="center">
  <img src="Images/Page-2.png" alt="P2" />
</p>

### 🎯 RFM Segmentation Insights (Last 12 months)

* **RFM performance is weak:** Avg score 2.04/5. Recency is acceptable (3.00) but **Frequency collapses (1.24)** and Monetary is low (1.86) → customers are reachable but not repeating.

* **Customers are reachable but inactive:** Recency indicates recent contact, but Frequency ≈ 1 confirms most customers bought only once in 2023.

* **Customer mix skews low-value:** New Customers (32.8%) and Promising + Hibernating + Lost (~60%) dominate; Champions are only 4.5%.

* **Revenue ≠ loyalty:** ~55% of revenue comes from New Customers and Promising (Frequency = 1); Champions contribute just 11.6% due to small size.

* **Core RFM failure = Frequency:** Low Monetary is driven by missing repeat purchases, not low spending power.

#### **Page 3: Retention Analysis (Cohort)**
<p align="center">
  <img src="Images/Page-3.png" alt="P3" />
</p>

### 📅 Retention (Cohort Analysis) Insights

* **Strong acquisition, weak early retention:** Month 1 retention of New Customers only ~0.4–5.6%; even best segments reach just ~15–17%.

* **One-time purchase dominates:** New, Promising, About to Sleep retain only at Month 0 → aligned with Frequency ≈ 1.

* **Potential Loyalists fail to convert:** Early retention peaks ~18% (Months 1–3) then drops fast → missed nurturing.

* **Champions perform best but are small:** Stable retention (~3–4% at Months 9–11) but only ~4.5% of customers.

* **Loyal segment negligible:** 1 customer (0.01%) → not meaningful for analysis.

### 💰 LTV Analysis Insights (Cohort-based, 4-year Lifecycle)

* **LTV grows slowly:** Repeat purchases exist but are delayed and weak.

* **Champions set the benchmark:** LTV ~216 → ~390 (Month 0–12), most sustainable.

* **Loyal value is abrupt:** Sharp LTV jump (~484 at Month 9) then flat.

* **Mid-tier underperform:** Promising (~227→260), Potential Loyalists (~115→166) → large unrealized LTV.

* **Churn after value extraction:** Hibernating & Lost still add LTV (~161→187; ~154→170), showing late disengagement cost.
---

### 🌎 Overall Insights – Summary

- The business is achieving **strong customer acquisition growth**, but **value creation per customer remains weak and inefficient**. Despite growth in total customers (+25.9%) and returning customers (+73.2%), repeat behavior rarely extends beyond the second purchase. Revenue growth is primarily **volume-driven rather than value-driven**, as revenue increases (+21.5%) alongside a decline in AOV (–6.9%), confirming that upsell, cross-sell, and basket expansion remain ineffective.
- Across RFM, retention, and LTV, a consistent pattern emerges: customers are **reachable but inactive** (Recency ≈ 3.0, Frequency ≈ 1). Early retention fails, mid-tier segments are not activated, and LTV accumulates slowly and late. While Champions show strong value behavior, they represent only ~4.5% of customers—leaving growth dependent on continued **acquisition rather than sustainable**, repeat-driven value.


🚨**THE BIGGEST PROBLEM**
👉 **Purchase Frequency is the core structural bottleneck.**
The business lacks an effective lifecycle engine to convert first-time buyers into early and repeated purchasers, resulting in:
  - Frequency collapse across nearly all segments
  - Weak Month 1–3 retention, where customer value should be established
  - Delayed and inefficient LTV realization

👉 Low Monetary performance in 2023 is **not a spending power issue**, but a direct consequence of missing repeat purchase behavior.

---

## 💡 D. Strategic Recommendations

- **1️⃣ Win the Second Purchase Early**

  - Focus on **Month 0–2**, where Frequency and retention drop fastest, using timely reminders, replenishment cues, and low-friction incentives.

- **2️⃣ Pull LTV Forward**

  - Accelerate repeat timing for **Promising and Potential Loyalists** to realize value earlier, instead of relying on slow LTV accumulation.

- **3️⃣ Scale Champion Behaviors**

  - Replicate Champions’ purchase cadence, bundles, and routines across mid-tier segments to grow the high-value customer base.

- **4️⃣ Prevent Early Dormancy**

  - Re-engage at-risk and Hibernating customers before **Recency collapses** and lifetime value stalls.

---

**Tools**: Power BI Desktop | Power Query | DAX | Excel  
**Techniques**: RFM Analysis | Cohort Analysis | Customer Segmentation | LTV Modeling  
**Industry**: E-Commerce | Retail | Beauty & Skincare