**Azure Databricks Medallion Architecture – Data Pipeline**

**Overview**

This project demonstrates an end-to-end data engineering pipeline built on Azure Databricks following the Medallion Architecture (Bronze → Silver → Gold) pattern.

The pipeline ingests Parquet files from Azure Blob Storage, performs transformations, and builds a fact table for analytics-ready data.

**Data Sources**
The following datasets are ingested from Azure Blob Storage:

1. Products
2. Customers
3. Orders
4. Regions

**Architecture**

<img width="1408" height="736" alt="image" src="https://github.com/user-attachments/assets/566c105c-f4d1-40be-8390-574ac49703a5" />



**Pipeline Flow**

**Bronze Layer – Ingestion**

Reads Parquet files from Azure Blob Storage using Databricks.
Uses a single notebook with a foreach loop to process multiple source datasets.
Basic data exploration and profiling, perfor schema inspection and record counts.

Data is written to the Bronze layer in Data Lake.

**Silver Layer – Transformation**

Reads Bronze data and applies data cleansing and business rules.
Writes data as Delta Tables to the Silver layer.
Ensures schema consistency and handles duplicates.
Converts Parquet → Delta format for efficient downstream processing.

**Gold Layer – Data Warehouse Modeling**

Applies Slowly Changing Dimensions (SCD) logic:
Customers: SCD Type 1 (overwrite updates)
Products: SCD Type 2 (historical tracking) using Delta Live Tables
Creates Order (Fact) table by joining the Silver Delta tables.



<img width="1000" height="800" alt="image" src="https://github.com/user-attachments/assets/da9b19e0-3341-47c8-a80c-6093ae13a347" />

