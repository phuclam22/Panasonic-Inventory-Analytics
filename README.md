# 📦 PanaSony Inventory Intelligence Dashboard

**End-to-End Inventory Management Data Pipeline & Power BI Dashboard**
---
## Executive Summary
The **PanaSony Inventory Intelligence Dashboard** is a comprehensive analytics solution engineered to provide real-time visibility into inventory operations across six global manufacturing facilities. 

This project bridges the gap between raw, disconnected ERP data and strategic decision-making. By consolidating 3,780 inventory records across six years (2020–2025) and applying robust data modeling, the solution empowers Supply Chain leadership to monitor $378.88M in capital tied up in inventory, proactively prevent stockouts, and optimize cross-regional stock rebalancing.

**Tech Stack Used:**
- **Database:** Microsoft SQL Server (SSMS)
- **ETL & Data Modeling:** T-SQL (Star Schema, Denormalization via Views, Table Variables)
- **Visualization:** Microsoft Power BI (DAX, Interactive Dashboards)

---

## Business Problem
PanaSony previously lacked a centralized, real-time view of its vast inventory portfolio (over 2.38M units on hand). Inventory managers relied on manual, weekly data extractions from 6 disconnected factory systems, leading to critical operational blind spots:
1. **Poor Availability Visibility:** The company couldn't quickly identify which factories or product categories were dragging down the overall availability average (77.82%), causing fulfillment delays.
2. **In-Transit Blind Spots:** Over 472K units were consistently in-transit without active monitoring, causing receiving bottlenecks and logistical uncertainty.
3. **Capital Misallocation:** Finance teams lacked real-time visibility into the $378.88M inventory value to strategically monitor high-value facilities like the Pune factory.

---

## Data Architecture & ETL Pipeline
The project utilizes a robust **Star Schema** data model, optimized for BI reporting.

### 1. Data Modeling
- **Fact Table:** `FactInventory` (Granular daily snapshot of inventory levels).
- **Dimension Tables:** `DimDate`, `DimFactory`, `DimProduct` (Providing analytical context such as geographic regions, business units, and product categories).

### 2. ETL Process (Extract, Transform, Load)
- Implemented **Surrogate Key Lookups** using in-memory Table Variables (`@InvBatch`) to efficiently map incoming alphanumeric source data (`FactoryCode`, `SKU`) to integer primary keys (`FactoryKey`, `ProductKey`), ensuring high insertion performance.
- Engineered a centralized denormalized view (`vw_PowerBI_FactInventorySummary`) that pre-aggregates key metrics (Sums and safe percentages) and flattens the schema. This pushes heavy computational loads to the SQL engine, resulting in a lightweight, lightning-fast Power BI dataset.

---

## Dashboard Design & Key Metrics
The Power BI solution is divided into four highly interactive pages, utilizing synchronized slicers (Year, Region, City) and custom DAX measures for dynamic analysis.

### Core DAX Measures Developed
- **Safe Division Ratios:** Used `DIVIDE()` to calculate `Available Percentage` and `Reserved Percentage`, preventing zero-denominator errors.
- **Row-Level Iteration:** Leveraged `AVERAGEX` to accurately calculate average availability across uneven factory scales.
- **Dynamic Conditional Formatting:** Built automated "Stock Health" flags classifying facilities as "Healthy," "Monitor," or "Critical" based on target thresholds.

### Report Pages
1. **Inventory Management (Overview):** High-level KPI cards and trendlines tracking the $378.88M portfolio and overall availability health.
2. **Stock Analysis:** Detailed physical volume breakdown (Available vs. Reserved vs. In-Transit) and a comprehensive Factory Performance Scorecard.
3. **Inventory Value:** Financial perspective mapping capital allocation across Business Units (e.g., Connected Solutions vs. Housing Systems).
4. **Factory View:** Deep-dive scatter plot matrix evaluating operational efficiency (On-Hand vs. Availability) sized by financial value.

---

## Business Impact & Strategic Value
Implementing this solution delivers significant, quantifiable value to the business:
- **95% Reduction in Reporting Time:** Transitions the organization from multi-day manual Excel consolidations to an instant, self-service real-time dashboard.
- **Proactive Risk Mitigation:** Highlights critical laggards (e.g., Tokyo Plant at 77.27% availability) allowing immediate intervention before stockouts occur.
- **Capital Optimization:** By identifying excessive reserved stock ratios (currently ~22.18%), the company can audit commitments to free up trapped capital and improve overall inventory turnover.

---

## Repository Structure
```text
📦 Panasonic-Inventory-Analytics
 ┃ 📜 DimDate.sql                   # Date dimension population
 ┃ 📜 DimFactory.sql                # Factory dimension population
 ┃ 📜 DimProduct.sql                # Product dimension population
 ┃ 📜 FactInventory_1_to_5.sql      # Fact table batch inserts & ETL logic
 ┃ 📜 FactInventorySummary.sql      # Denormalized View creation for Power BI
 ┃ 📜 Inventory Intelligence Dashboard.pbix  # The complete Power BI file
 ┣ 📜 LICENSE
 ┗ 📜 README.md                       # Project documentation
