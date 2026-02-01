# E-Commerce Customer Intelligence Platform
## Problem Statement
E-commerce platforms generate large volumes of transactional and behavioral data, yet identifying high-value customers early in their lifecycle remains a significant challenge.

While a small percentage of customers typically contribute a majority of revenue, this information is only known after an extended purchase history. Traditional rule-based systems fail to capture complex customer behavior patterns and cannot reliably predict future customer value.

This project aims to build an end-to-end AI-powered analytics platform that enables early identification of high-value customers using scalable data engineering and machine learning capabilities provided by Databricks Lakehouse.

## Business Objectives
* Identify customers who generate disproportionately high revenue
* Predict high-value customers early using behavioral signals
* Understand key drivers influencing customer value
* Provide actionable insights for marketing, retention, and operations teams
* Build a production-ready, automated analytics platform

## Dataset
### Olist Brazilian E-Commerce Dataset (https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce?select=olist_customers_dataset.csv)
The dataset represents a real-world multi-table transactional system and includes:
* Orders
* Order Items
* Payments
* Customers
* Products
* Sellers
* Reviews
* Geolocation
* Product Category Translation

## Architecture Overview
<img width="239" height="968" alt="Untitled Diagram drawio" src="https://github.com/user-attachments/assets/2a4319c6-c4e7-463f-9fe3-5d7d98580cc2" />

## Bronze Layer – Raw Ingestion
### Purpose
- Preserve raw source data
- Enable auditability and reprocessing

### Key Characteristics
- Explicit schema enforcement
- Metadata columns (_ingested_at, _source_file)
- Unity Catalog managed Delta tables
- No transformations applied

### Bronze Tables
- brz_orders
- brz_customers
- brz_order_items
- brz_payments
- brz_reviews
- brz_products
- brz_sellers
- brz_geolocation
- brz_product_category_translation

## Silver Layer – Data Transformation
The Silver layer converts raw datasets into analytics-ready structures.

### Key Transformations
- Duplicate handling on business keys
- Timestamp standardization
- Removal of ingestion metadata
- Multi-table joins
- Category translation enrichment
- Delivery performance metrics

### Derived Metrics
- Delivery days
- Delivery delay days
- Aggregated payment values
- Average review score

### Output Tables
- slv_order_fact
- slv_customers
- slv_products

## Gold Layer – Business Analytics & ML Datasets
### Business KPIs
- Daily revenue
- Monthly revenue
- Order volume
- Product category performance

### Customer Metrics
- Total orders
- Purchase frequency
- Recency of purchase
- Delivery experience indicators
- Customer satisfaction metrics

## RFM Feature Engineering
Customer behavior is modeled using RFM methodology:
- Recency – Days since last purchase
- Frequency – Number of completed orders
- Monetary – Lifetime spend (used only for labeling)

## High-Value Customer Definition
High-value customers are defined as those whose lifetime spending lies above the 75th percentile of total customer spend.

This percentile-based approach:
- Avoids arbitrary thresholds
- Adapts dynamically to dataset distribution
- Reflects real-world customer value concentration

## Machine Learning Approach
### ML Objective
Binary classification:
- 1 → High-value customer
- 0 → Regular customer

## Target Leakage Prevention
Monetary-based features were used only to create the label and were explicitly excluded from model inputs to prevent data leakage.

Excluded features:
- Total spend
- Monetary value
- Average order value

This ensures predictions rely solely on information available before customer value is fully realized.

## Features Used for Modeling
- Recency days
- Purchase frequency
- Average review score
- Average delivery days
- Average delivery delay

These features represent early behavioral and experience signals.

## Model Performance
| Model               | ROC-AUC |
| ------------------- | ------- |
| Logistic Regression | 0.60   |
| Random Forest       | 0.61   |
| Gradient Boosting   | 0.61   |

While model performance is moderate, these results reflect realistic constraints:
- No future monetary information is used
- Predictions rely only on early behavioral signals
- Customer lifetime value is inherently difficult to predict

## SQL Analytics Dashboard

<img width="645" height="281" alt="image" src="https://github.com/user-attachments/assets/bbd550ba-e5f5-4c70-b645-de3eb06da3ed" />

The Databricks SQL dashboard provides:
- Revenue trends over time
- High-value vs regular customer distribution
- Revenue contribution by segment
- Delivery performance impact analysis
- Top product categories by revenue

## Workflow Orchestration
Databricks Workflows automate the complete pipeline:

<img width="511" height="46" alt="image" src="https://github.com/user-attachments/assets/da258e11-40ad-4053-a49e-9471fb0e8e82" />

<img width="1060" height="733" alt="Screenshot 2026-02-01 173936" src="https://github.com/user-attachments/assets/55f05824-ef91-404f-b198-d687b09cc979" />

## Business Impact
This platform enables:
- Early identification of high-value customers
- Targeted marketing and loyalty campaigns
- Improved retention strategies
- Data-driven operational optimization
- Scalable AI-powered decision making
- 
## Conclusion

This project demonstrates how Databricks unifies data engineering, analytics, and machine learning into a single scalable Lakehouse platform.

By combining robust data architecture with realistic machine learning practices and strong governance, the solution delivers actionable customer intelligence suitable for real-world enterprise environments.
