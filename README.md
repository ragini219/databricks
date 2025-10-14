**Azure Databricks Medallion Architecture – Parquet Data Pipeline**

**Overview**

This project demonstrates an end-to-end data engineering pipeline built on Azure Databricks following the Medallion Architecture (Bronze → Silver → Gold) pattern.

The pipeline ingests Parquet files from Azure Blob Storage, performs transformations, and builds a fact table for analytics-ready data.

**Data Sources**
The following datasets are ingested from Azure Blob Storage:

1. Products
2. Customers
3. Orders
4. Regions

Each dataset is stored as Parquet files in a dedicated container/path.

| Layer      | Purpose                                                                           | Format  |
| ---------- | --------------------------------------------------------------------------------- | ------- |
| **Bronze** | Raw ingestion layer. Loads data directly from source files.                       | Parquet |
| **Silver** | Cleansed and conformed data. Includes business transformations and deduplication. | Delta   |
| **Gold**   | Curated, analytics-ready data models (e.g., SCDs and fact tables).                | Delta   |

**Architecture**

![Uploading Gemini_Generated_Image_421dph421dph421d.png…]()


**Pipeline Flow**

**Bronze Layer – Ingestion**

Reads Parquet files from Azure Blob Storage using Databricks.
Uses a single notebook with a foreach loop to process multiple source datasets.
Basic data exploration and profiling performed, schema inspection and record counts.

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

<img width="1918" height="913" alt="image" src="https://github.com/user-attachments/assets/67156f9d-1c92-416b-b1f9-23712bb37b35" />


<img width="1000" height="800" alt="image" src="https://github.com/user-attachments/assets/da9b19e0-3341-47c8-a80c-6093ae13a347" />

