# Zepto Inventory Analysis using AWS (S3, Glue, Athena)

## 1. Project Overview

This project demonstrates the design and implementation of a cloud-based data pipeline for analyzing inventory data from Zepto. The dataset, sourced from Kaggle, was processed using AWS services to enable scalable storage, automated schema detection, and SQL-based analysis.

The objective of this project is to derive actionable insights related to inventory efficiency, pricing strategy, stock availability, and potential revenue risks.

---

## 2. Dataset

* Source: Kaggle (Zepto Inventory Dataset)
* Format: CSV
* Key Fields:

  * Category
  * Product Name
  * MRP
  * Discount Percentage
  * Discounted Selling Price
  * Available Quantity
  * Out of Stock Flag
  * Weight

---

## 3. Technology Stack

* Amazon S3 – Data storage (data lake)
* AWS Glue – Data catalog and schema inference
* Amazon Athena – Serverless SQL query engine
* SQL – Data analysis

---

## 4. Architecture

Kaggle Dataset → Amazon S3 (raw layer) → AWS Glue (crawler & catalog) → Amazon Athena (SQL queries)

---

## 5. Data Pipeline Implementation

### Step 1: Data Ingestion

* Uploaded raw CSV dataset to Amazon S3
* Designed folder structure:

  * `raw/` for source data
  * `processed/` for transformed outputs

### Step 2: Data Cataloging

* Created AWS Glue database (`zepto_db`)
* Configured and executed a Glue crawler to:

  * Scan S3 data
  * Infer schema
  * Create table (`raw`)

### Step 3: Data Querying

* Configured Amazon Athena with S3 output location
* Queried data directly from S3 using SQL without provisioning a database

---

## 6. Key Business Analyses

### 6.1 Inventory Inefficiency Analysis

Identified categories with high stock levels but low average selling price, indicating inefficient capital allocation.

### 6.2 Stockout Risk Detection

Detected products with very low available quantity that are likely to go out of stock soon.

### 6.3 Revenue Leakage Estimation

Estimated potential revenue loss due to out-of-stock products using average selling price.

### 6.4 Discount Strategy Evaluation

Analyzed whether high discounts are being applied effectively relative to product pricing.

### 6.5 Inventory Value Concentration

Identified high-value inventory items and categories to understand capital concentration risk.

---

## 7. Sample SQL Queries

### Stockout Risk Detection

```sql
SELECT name, category, availablequantity
FROM raw
WHERE availablequantity < 10
  AND outofstock = false;
```

### Revenue Leakage Estimation

```sql
SELECT category,
       COUNT(*) AS out_of_stock_products,
       AVG(discountedsellingprice) AS avg_price,
       COUNT(*) * AVG(discountedsellingprice) AS potential_loss
FROM raw
WHERE outofstock = true
GROUP BY category;
```

### Inventory Value by Category

```sql
SELECT category,
       SUM(discountedsellingprice * availablequantity) AS total_value
FROM raw
GROUP BY category
ORDER BY total_value DESC;
```

---

## 8. Key Learnings

* Built an end-to-end data pipeline using AWS services
* Understood schema inference using AWS Glue crawlers
* Performed serverless querying using Amazon Athena
* Applied SQL for business-driven inventory analysis
* Gained exposure to cloud-based data architecture

---

## 9. Future Improvements

* Develop a Power BI dashboard for visualization
* Automate ETL workflows using AWS Glue jobs
* Implement incremental data processing
* Integrate real-time data ingestion

---

## 10. Repository Structure

```
zepto-aws-inventory-analysis/
│
├── README.md
├── queries.sql
├── screenshots/
└── dataset/ (optional sample)
```
