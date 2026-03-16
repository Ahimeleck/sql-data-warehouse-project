# Modern Data Warehouse Project (Medallion Architecture)

## Overview

This project implements a **modern Data Warehouse using the Medallion Architecture (Bronze → Silver → Gold)** to ingest, transform, and model data from multiple operational systems.

The pipeline integrates **CRM and ERP data sources** and produces **analytics-ready datasets using a Star Schema** designed for reporting and business intelligence.

This project demonstrates core **Data Engineering practices**, including:

- Data ingestion pipelines
- Data cleaning and normalization
- Data integration from multiple systems
- Dimensional modeling
- ETL observability and error handling

The final output is a **business-ready analytical model for sales analysis**.

---

# Architecture

The warehouse follows the **Medallion Architecture**, which organizes data into three layers:

| Layer | Purpose |
|------|------|
| Bronze | Raw ingestion layer |
| Silver | Cleaned and standardized data |
| Gold | Business-ready analytical models |

This layered design improves **data reliability, traceability, and scalability**.

![Architecture](docs/data_flow.png)

---

# Data Pipeline

The pipeline processes data through the following stages:

```
CRM + ERP Sources
      ↓
Bronze Layer (Raw ingestion)
      ↓
Silver Layer (Cleaning & Standardization)
      ↓
Gold Layer (Star Schema for Analytics)
```

---

# Data Sources

Two operational systems are integrated into the warehouse.

## CRM (Customer Relationship Management)

Contains transactional sales data.

Tables:

- `crm_sales_details`
- `crm_prd_info`
- `crm_cust_info`

---

## ERP (Enterprise Resource Planning)

Provides additional product and customer attributes.

Tables:

- `erp_cust_az12`
- `erp_loc_a101`
- `erp_px_cat_g1v2`

---

# Data Model

The Gold layer implements a **Star Schema** optimized for analytical workloads.

![Data Model](docs/data_model.png)

## Fact Table

`fact_sales`

Measures:

- `sales_amount`
- `quantity`
- `price`

Sales calculation:

```sql
sales = quantity * price
```

---

## Dimension Tables

### dim_customers

Contains:

- customer identifiers
- demographic attributes
- customer location

### dim_products

Contains:

- product attributes
- category and subcategory
- product line
- maintenance indicator

---

# ETL Pipeline

## Bronze Layer

The Bronze layer stores **raw data exactly as received from source systems**.

### Characteristics

- No transformations applied
- Full load strategy (`TRUNCATE + INSERT`)
- Raw data preserved for traceability

### Data Ingestion

Data is loaded using **Bulk Insert** for efficient ingestion.

A **stored procedure** automates the ingestion process across tables.

Features implemented:

- `CREATE OR ALTER PROCEDURE`
- Debug logging using `PRINT`
- Error handling with `TRY / CATCH`
- ETL duration monitoring

Example execution tracking:

```sql
DECLARE @start_time DATETIME
DECLARE @end_time DATETIME

DATEDIFF(SECOND, @start_time, @end_time)
```

This allows monitoring:

- table-level load duration
- full batch execution time

---

## Silver Layer

The Silver layer contains **cleaned and standardized data**.

Transformations include:

- Data cleansing
- Data normalization
- Duplicate removal
- Text standardization
- Metadata enrichment

### Duplicate Handling

Duplicates are removed using `ROW_NUMBER()`.

```sql
ROW_NUMBER() OVER(
PARTITION BY id
ORDER BY create_date DESC
)
```

Only the most recent record is retained.

### Metadata Tracking

A metadata column tracks when records are inserted into the warehouse:

```sql
dwh_create_date DATETIME2 DEFAULT GETDATE()
```

This improves **data lineage and auditing**.

---

## Gold Layer

The Gold layer contains **analytics-ready datasets designed for reporting**.

This layer applies:

- Data integration
- Business logic
- Dimensional modeling

Outputs include:

- Fact tables
- Dimension tables
- Analytical views

---

# Naming Conventions

The project follows consistent **snake_case naming conventions**.

Example:

```sql
customer_id
order_number
product_name
sales_amount
```

Tables follow the pattern:

```
schema.source_table_name
```

Examples:

```
bronze.crm_sales_details
silver.crm_cust_info
gold.fact_sales
```

This convention improves **data lineage visibility across layers**.

---

# Technologies Used

- SQL Server
- T-SQL
- Medallion Architecture
- Dimensional Modeling
- Bulk Insert
- Stored Procedures
- Git

---

# Key Data Engineering Concepts Demonstrated

This project demonstrates practical experience with:

- Data warehouse architecture
- ETL pipeline design
- Data cleansing and normalization
- Star schema modeling
- Data integration across systems
- ETL monitoring and observability

---

# About Me

Hi, I'm **Ahimelec Gomez**.

I am a **Data professional focused on Data Engineering**, with experience in:

- SQL
- Data Warehousing
- Data Modeling
- Analytics Engineering concepts

I enjoy designing **data pipelines, data models, and scalable analytics systems**.

I'm currently continuing to develop my skills in **cloud data platforms and modern data engineering tools**.







