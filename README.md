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

### 3. 📈 Key Insights and Visualizations

Developed interactive three-page report:

#### **Page 1: Customer Overview** (focused on 2023)
<p align="center">
  <img src="Images/Page-1.png" alt="P1" />
</p>

### 📍 Customer Overview Insights (2023)

* **Customer growth is strong but shallow:** Total customers +24.7%, returning customers +76.5%, yet repeat behavior rarely persists beyond the second purchase.
* **Revenue grows by volume, not value:** Revenue +23.8% while AOV –2.5%, showing limited upsell, cross-sell, and basket expansion.
* **Purchase frequency is structurally low:** 2.3 purchases/year (~every 5 months), far below the 1–3 month skincare repurchase cycle.
* **Growth is uneven by region:** SEA & Oceania grow fastest (~2× others), while Western Europe & Central America still account for >50% of customers and must remain stable.

#### **Page 2: Segmentation Analysis (RFM)** 
<p align="center">
  <img src="Images/Page-2.png" alt="P2" />
</p>

### 🎯 RFM Segmentation Insights (Last 12 months)

* **Overall RFM quality is weak:** Average RFM 2.04/5, driven by very low Frequency (1.24) and Monetary (1.87), despite acceptable Recency (3.00).
* **Customers are reachable but inactive:** Recency indicates recent contact, but Frequency ≈ 1 confirms most customers bought only once in 2023.
* **Customer base skews low-value:** New Customers alone = 32.2% of customers; Promising + Hibernating + Lost ≈ 61%, while Champions are only 4.24%.
* **Revenue does not reflect loyalty:** Promising and New Customers together generate >55% of revenue, while Champions contribute just 11.6% due to small size.
* **Core RFM failure = Frequency collapse:** Low Monetary is a consequence of missing repeat purchases, not weak spending capacity.

#### **Page 3: Retention Analysis (Cohort)**
<p align="center">
  <img src="Images/Page-3.png" alt="P3" />
</p>

### 📅 Retention (Cohort Analysis) Insights

* **Large cohorts but low retention:** Month 1–3 only 2.7–4% → strong acquisition, weak repurchase.
* **Some cohorts show recovery:** Likely from seasonal demand or reactivation.
* **One-time purchase dominates:** New Customers, Promising, and About to Sleep retain only at Month 0, fully aligning with Frequency ≈ 1.
* **Potential Loyalists show intent but are not activated:** Slightly better early retention, but no sustained lifecycle engagement.
* **Champions retain best but are too few:** Strongest retention profile, yet only 4.24% of customers—insufficient to stabilize the system.

### 💰 LTV Analysis Insights (Cohort-based, 4-year Lifecycle)

* **True LTV exists but materializes late:** Across four years, most segments accumulate value gradually, revealing delayed repeat behavior invisible in a 1-year view.
* **Champions set the benchmark:** LTV grows steadily (~344 → ~625 by Month 12), reflecting consistent repeat purchasing and strong lifecycle engagement.
* **Loyal customers front-load value:** High early LTV (~717–859) but quick saturation, indicating strong onboarding but weak long-term deepening.
* **Mid-tier segments grow slowly and inefficiently:** Promising and Potential Loyalists accumulate moderate LTV late, leaving significant unrealized value.
* **Churn happens after value is created:** Hibernating and Lost still reach ~275–281 LTV, highlighting the cost of late disengagement.

---

### 🌎 Overall Insights – Summary

The store is growing through **strong acquisition**, but **customer value creation is inefficient and delayed**. Most customers purchase once and disengage early, while meaningful lifetime value—where it exists—accumulates too late in the lifecycle. Revenue is therefore volume-driven, retention remains weak, and growth depends heavily on reacquisition and late reactivation rather than sustainable repeat behavior.

🚨**THE BIGGEST PROBLEM**
👉 **Purchase frequency is the strategic bottleneck.**
The store lacks an effective lifecycle engine to convert first-time buyers into early repeat customers, causing Frequency collapse, early retention failure, and delayed LTV realization across nearly all segments.

---

## 💡 D. Strategic Recommendations

- **1️⃣ Win the Second Purchase Early**

Prioritize Month 0–2 conversion with targeted reminders, replenishment cues, and low-friction incentives—where retention and Frequency drop the fastest

- **2️⃣ Pull LTV Forward, Not Just Up**

Focus on accelerating repeat timing for Promising and Potential Loyalists to compress LTV into earlier months, rather than relying on slow multi-year accumulation.

- **3️⃣ Scale Champions’ Behaviors**

Replicate Champions’ purchase patterns (cadence, bundles, routines) across mid-tier segments to expand high-quality customer share.

- **4️⃣ Intervene Before Customers Go Dormant**

Proactively re-engage Hibernating and at-risk customers earlier, before Recency collapses and historical LTV stops growing.

---

**Tools**: Power BI Desktop | Power Query | DAX | Excel  
**Techniques**: RFM Analysis | Cohort Analysis | Customer Segmentation | LTV Modeling  
**Industry**: E-Commerce | Retail | Beauty & Skincare