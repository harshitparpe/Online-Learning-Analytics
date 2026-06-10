# Online Learning Analytics Platform

> End-to-end cloud data engineering pipeline built on Microsoft Azure using Medallion Architecture. Processes raw online learning survey data through automated ETL, data quality remediation, cloud-native storage, and business intelligence dashboards.

## Badges

![Azure](https://img.shields.io/badge/Azure-0089D6)
![Databricks](https://img.shields.io/badge/Databricks-FF3621)
![PySpark](https://img.shields.io/badge/PySpark-E25A1C)
![PowerBI](https://img.shields.io/badge/PowerBI-F2C811)
![Synapse](https://img.shields.io/badge/Synapse-0078D4)
![Parquet](https://img.shields.io/badge/Parquet-50ABF1)

## Links

[Architecture](#architecture) ·
[Pipeline Flow](#data-pipeline) ·
[Dashboard Insights](#dashboard-insights)

## Features

* End-to-end Medallion Architecture implementation (Bronze → Silver → Gold)
* Automated ETL pipeline using Azure Data Factory and Azure Databricks
* Processing and validation of 1,225+ learner records
* Resolution of 7 major categories of data quality issues
* PySpark-based cleaning, transformation, and feature engineering
* Partitioned Parquet storage for scalable analytics workloads
* Azure Synapse serverless SQL analytics using OPENROWSET
* Interactive Power BI dashboards for business intelligence
* Cloud-native architecture using Azure services only
* Production-style layered data architecture following industry best practices

## Project Metrics

| Metric                    | Value               |
| ------------------------- | ------------------- |
| Records Processed         | 1,225+              |
| Data Quality Issues Fixed | 7 Categories        |
| Data Quality Improvement  | ~95%                |
| Storage Format            | Partitioned Parquet |
| Cloud Platform            | Microsoft Azure     |
| Dashboard Platform        | Power BI            |

## Tech Stack

| Category          | Technologies                            |
| ----------------- | --------------------------------------- |
| Cloud Platform    | Microsoft Azure                         |
| Storage           | Azure Data Lake Storage Gen2            |
| ETL Orchestration | Azure Data Factory                      |
| Data Processing   | Azure Databricks, Apache Spark, PySpark |
| Analytics         | Azure Synapse Analytics                 |
| Visualization     | Microsoft Power BI                      |
| Data Format       | CSV, Parquet                            |
| Query Engine      | Serverless SQL Pool                     |

## Architecture

![Azure Medallion Architecture Pipeline](assets/architecture.png)

The platform follows the Medallion Architecture pattern to progressively improve data quality and business value.

### Bronze Layer

* Raw CSV ingestion
* Immutable source storage
* Audit and recovery baseline
* Azure Data Factory ingestion

### Silver Layer

* Data cleansing and standardization
* Duplicate removal
* Missing value handling
* Feature engineering
* Partitioned Parquet generation

### Gold Layer

* Business-ready aggregations
* Dimension-based partitions
* Optimized analytics datasets
* Power BI consumption layer

## Data Pipeline

### Data Quality Remediation

| Issue               | Resolution                        |
| ------------------- | --------------------------------- |
| Missing Values      | Median and categorical imputation |
| Duplicates          | Record deduplication              |
| Inconsistent Labels | Standardization rules             |
| Outliers            | Domain-based filtering            |
| Formatting Errors   | String normalization              |
| Invalid Data Types  | Explicit schema enforcement       |
| Case Variations     | Canonical transformations         |

### Feature Engineering

Three analytical features were generated:

* **engagement_score** – learner engagement metric
* **satisfaction_group** – Low / Medium / High segmentation
* **completed_binary** – standardized completion indicator

## Dashboard Insights

### Executive Analytics

* Australia and UK demonstrate the highest course completion rates
* Master's degree learners achieve the strongest quiz performance
* Device usage remains evenly distributed across Desktop, Mobile, and Tablet
* Moderate weekly study hours correlate with peak academic performance
* Completion rates remain consistent across education levels

### Behavioral Analytics

* Login frequency is a stronger performance predictor than study hours
* Satisfaction and completion rates show weak correlation
* Gender distribution remains balanced across the dataset
* Regional satisfaction differences are observable across countries
* Mobile-first learners exhibit higher engagement metrics

## Project Structure

```text
online-learning-analytics/
│
├── data/
│   ├── raw/
│   ├── silver/
│   └── gold/
│
├── notebooks/
│   ├── 01_bronze_ingest
│   ├── 02_silver_clean
│   └── 03_gold_aggregate
│
├── synapse/
│   └── sql_queries/
│
├── powerbi/
│   └── dashboards/
│
├── assets/
│   └── architecture.png
│
└── README.md
```

## Pipeline Workflow

1. Raw survey data uploaded to Azure Data Lake Storage Gen2
2. Azure Data Factory orchestrates notebook execution
3. Databricks performs cleaning and transformation
4. Silver datasets stored as partitioned Parquet
5. Gold business aggregates generated
6. Synapse executes serverless SQL queries
7. Power BI consumes curated analytics datasets

## Future Enhancements

* Real-time streaming analytics with Azure Event Hub
* Automated scheduling through ADF triggers
* ML-based course completion prediction
* Predictive analytics dashboards
* Anomaly detection for learner behavior
* Multi-source ingestion pipelines

## Academic Information

| Field       | Value                          |
| ----------- | ------------------------------ |
| Subject     | Data Engineering & Analysis    |
| Semester    | 6th Semester                   |
| Degree      | B.Tech Information Technology  |
| Institution | Delhi Technological University |

## Team

* Harshit Parpe
* Harsh Gautam Jha

Guide: Prof. Khushbu Gupta

