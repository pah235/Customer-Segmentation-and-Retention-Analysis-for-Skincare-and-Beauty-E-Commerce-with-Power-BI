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

- **Strong customer base growth with improved retention**: Total customers increased 24.72% to 7,624 and returning customers surged 76.45% to 2,990. This shows the store is both attracting new customers well and significantly improving customer retention capabilities.

- **Revenue growth but declining AOV**: Revenue increased 23.82% to \$2.19M while AOV decreased 2.54% to \$125. This reflects growth coming from customer volume, not order value, and the store needs to push upsell/cross-sell to raise AOV.

- **Purchase frequency remains low**: Purchase Frequency is only 2.30 times/year (+1.87%), equivalent to customers repurchasing about once every 5 months – lower than the 1-3 month skincare cycle. This demonstrates that purchase frequency is an important growth lever but has not been fully utilized.

- **Southeast Asia & Oceania show fastest growth**: SEA increased 36.69% and Oceania increased 34.07%, nearly double the rate in Western Europe & Central America. This shows these two regions should be prioritized for expansion investment.

- **Western Europe & Central America remain the foundation**: Western Europe accounts for 27.27% and Central America 26.71%, meaning over 50% of total customers. This shows the store still needs to maintain stability in these two core markets while expanding to new regions.

#### **Page 2: Segmentation Analysis (RFM)** (focused on 2023)
<p align="center">
  <img src="Images/Page-2.png" alt="P2" />
</p>

### 🎯 RFM Segmentation Insights

- **Customer base heavily skewed toward one-time buyers**: New Customers account for 28.31% while Champions are only 25.58%. This shows the conversion rate from new to loyal customers is low and needs improvement in onboarding & early retention.

- **Promising is a potential segment but not yet activated**: This group accounts for 25.04% of customer base and has stronger growth potential but has not been properly activated. This shows the need to build specific activation strategies for this group.

- **Loyal segment too small for industry scale**: Loyal accounts for only 8.33%, low for the skincare industry which has replenishment characteristics. This shows customers are not being maintained for repeat purchases frequently enough.

- **Frequency is the major weakness across entire customer base**: Frequency is only at 2.05/5 – lowest in RFM, although Recency and Monetary are fairly stable. This shows customers lack reasons to return frequently, possibly due to lack of repurchase triggers or loyalty programs.

- **Need Attention & About to Sleep are risk signals**: Need Attention accounts for 3.28% and About to Sleep 1.41%, totaling ~4.7% of customers about to churn. This warns the store needs to deploy win-back campaigns early.

- **Body care dominates, Face care is neglected**: Body care has 4.7K customers while Face care has only 1K. This creates a large cross-sell opportunity, as Face care typically has higher purchase frequency and profit margins.

#### **Page 3: Retention Analysis (Cohort)**
<p align="center">
  <img src="Images/Page-3.png" alt="P3" />
</p>

### 📅 Retention (Cohort Analysis) Insights

- **Large cohorts but low retention**: Sep-23 cohort has 837 customers but Month 1-3 retention only 2.7%-4%. This shows that despite large customer influx, return rate remains limited.

- **Some cohorts show "recovery" patterns**: Jan-23 cohort increased retention from 3.8% to 5.15% at Month 10-11 and Apr-23 maintained ~4% at Month 7-8. This shows possible impact from seasonal demand or reactivation campaigns.

- **Champions have strongest retention**: With Month 1 at 5.85% and Month 2 at 6.31%, even Month 5-6 still ~4%, this is the segment with clearest and most stable loyal behavior.

- **Loyal & Need Attention have very low retention**: Loyal drops to <1% from Month 6 onward and Need Attention only 0.4-2.8%. This indicates high churn risk and needs programs to maintain this group.

- **Potential Loyalists show development potential**: Month 1-2 retention higher than Loyal (3.9% → 5.54%), showing this group should be prioritized for activation to upgrade to Loyal/Champions.

- **Some segments lack retention data after Month 0**: Promising, New Customers and About to Sleep only have 100% at Month 0, reflecting need for more time to assess return capability.

### 💰 LTV Analysis Insights

- **Three "frozen" segments show no LTV growth over 12 months**: New Customers (\$49.95), Promising (\$476.60) and About to Sleep (\$79.73) all show no increase after initial purchase. This shows they are not being properly reactivated.

- **Champions have highest ROI**: With LTV of \$495.68, store can spend CAC \$400+ and still be profitable. This confirms this is the most investment-worthy segment.

- **Loyal customers plateau after 9 months**: LTV grows very slowly (+0.4% in last 3 months), showing this segment lacks motivation to purchase more.

- **Potential Loyalists are "hidden gems"**: Although LTV is only \$56, the growth rate of +26.7% is second highest after Champions, proving this group has long-term growth potential.

- **Massive LTV gap**: Champions have LTV near \$500 – 10x the New group (~$50), showing strong differentiation in customer value.

- **Uneven LTV growth**: Champions grow strongly in months 1-6 then slow down; Loyal grows steadily but slowly; Potential Loyalists grow slightly but continuously. This reflects different purchasing behaviors across segments.

### 🌎 Overall Insights – Summary

- **Growth depends on new customers**: Despite strong customer base and revenue growth, declining AOV and frequency of only ~2.3 times/year shows the store is not yet effectively leveraging existing customers.

- **Customer structure imbalanced**: New Customers outnumber Champions, Loyal only 8.33%. This shows the conversion journey from New to Loyal/Champions is weak.

- **Potential segments not properly nurtured**: Promising and Potential Loyalists have capability to bring high value but have not been activated. This is an opportunity for rapid growth.

- **Retention weak and unstable**: Large cohorts have Month 1-3 retention only 2-4% and many cohorts show U-shape indicating post-purchase care funnel is not standardized.

- **RFM shows massive customer value differentiation**: Champions LTV is 10x New; many segments show no LTV growth over 12 months, proving need to focus on right segments instead of just increasing traffic.

- **Growth markets lie outside traditional regions**: SEA & Oceania grow fastest, showing need to prioritize expansion in emerging markets.

- **Body care dominates but Face care is neglected**: Gap of 4.7K vs 1K creates large opportunity for cross-sell and margin increase.

---

## 💡 D. Strategic Recommendations

Based on the analysis, the store should focus on **7 main strategic directions** to improve business performance:

### 1. Activate 3 "frozen" segments (53% of customer base)
- **New Customers**: Deploy strong onboarding journey in first 90 days with email nurturing, incentives and product education
- **Promising**: Analyze reasons for "freezing" and build targeted activation campaigns
- **About to Sleep**: Win-back campaigns with attractive offers and surveys to understand reasons for stopping purchases

### 2. Increase purchase frequency (from 2.3 to 3.5+ times/year)
- Deploy **subscription model** for consumable products with attractive incentives
- Build **smart replenishment reminders** based on product lifecycle
- Develop **loyalty program** with tiers to reward repeat purchases
- Create **urgency and scarcity** through limited-time offers and flash sales

### 3. Improve AOV (from \$125 to \$150+)
- **Upsell strategies**: Product recommendations, "frequently bought together"
- **Cross-sell campaigns**: Especially Body care → Face care
- **Bundle deals**: Skincare routines with discounts
- **Minimum order incentives**: Free shipping/gifts for orders above threshold

### 4. Optimize retention by cohort-specific patterns
- Analyze and **replicate success factors** of April cohort (best retention)
- Apply **adaptive timing** for campaigns based on peak retention month of each cohort
- Leverage **U-shape recovery pattern** with seasonal reactivation campaigns in Q4
- Standardize **customer journey** to reduce retention volatility

### 5. Scale high-value segments
- **Increase CAC investment** for Champion-profile prospects (can spend up to $200-400)
- Develop **referral program** leveraging Champions (LTV $496)
- Create **Champion pathway** to convert Potential Loyalists faster
- **VIP programs** to maintain and grow Loyal segment

### 6. Strategic geographic expansion
- **Double down** on Southeast Asia and Oceania (growth +35%)
- Localization: language, payment methods, logistics
- Local partnerships and influencer marketing
- Maintain stable growth in Western Europe and Central America

### 7. Optimize product mix
- **Cross-sell Face care** to 4.7K Body care customers
- Educational content about complete skincare routines
- Bundle promotions combining categories
- Analyze barriers to adoption and address

---

## 🚀 Conclusion

This project demonstrates end-to-end Business Intelligence development using Power BI, from stakeholder requirements gathering through data modeling to dashboard storytelling and strategic recommendations.

**Key Outcomes**:
- Identified **Frequency (2.05/5)** as the primary growth lever
- Discovered **53% of customer base "frozen"** (New, Promising, About to Sleep) requiring urgent activation
- Quantified **massive value gap** between segments (Champions LTV 10x New Customers)
- Provided **7 strategic directions** specifically to improve retention, frequency and LTV
- Built **analytical framework** scalable for ongoing monitoring
---

**Tools**: Power BI Desktop | Power Query | DAX | Excel  
**Techniques**: RFM Analysis | Cohort Analysis | Customer Segmentation | LTV Modeling  
**Industry**: E-Commerce | Retail | Beauty & Skincare