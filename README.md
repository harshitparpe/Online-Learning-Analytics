# 🎓 Online Learning Analytics — Azure Cloud Data Pipeline

> **End-to-end Medallion Architecture pipeline on Microsoft Azure** — transforming raw, inconsistent survey data into actionable business insights through automated ETL, cloud-native storage, and interactive Power BI dashboards.

![Azure](https://img.shields.io/badge/Microsoft_Azure-0089D6?style=for-the-badge&logo=microsoft-azure&logoColor=white)
![Databricks](https://img.shields.io/badge/Azure_Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-E25A1C?style=for-the-badge&logo=apache-spark&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Parquet](https://img.shields.io/badge/Parquet-50ABF1?style=for-the-badge&logo=apache&logoColor=white)

---

## 📊 Project Highlights

| Metric | Value |
|--------|-------|
| 📁 Raw Records Processed | 1,225+ |
| 🧹 Data Quality Issues Fixed | 7 error types |
| 📈 Data Quality Gain | ~95% |
| 🗄️ Storage Format | Parquet (partitioned) |
| ☁️ Cloud Platform | Microsoft Azure |

---

## 🧠 Problem Statement

Organizations generating data from online learning platforms often face **raw data that is incomplete, inconsistent, and unstructured**. Traditional processing methods lack the scalability and automation needed to clean and manage such data efficiently.

**Critical gaps this pipeline addresses:**
- No unified platform for end-to-end data lifecycle management
- Limited automation in data cleaning and preprocessing
- Difficulty scaling analytics workflows with growing data volume
- Lack of separation between raw, clean, and aggregated data layers

---

## 🏗️ Architecture — Medallion Pattern

```
SOURCE                 INGEST                    TRANSFORM
────────────────────────────────────────────────────────────────
online_learning     →  ADLS Gen2 (Raw)  →  Databricks Notebook 1
dirty.csv              Azure Data Factory        (Bronze – ingest)
[1225 rows]                                       ↓
                                           Databricks Notebook 2
                                           (Silver – clean data)
                                                  ↓
                                           Databricks Notebook 3
                                           (Gold – aggregate)

QUERY                               SERVE
────────────────────────────────────────────────────────────────
Azure Synapse Analytics  →  Power BI Interactive Dashboards
(Serverless SQL on Parquet)
```

### 🥉 Raw Zone
Original dirty CSV stored **as-is** in ADLS Gen2. Unmodified audit baseline ingested via ADF copy activity.

### 🥈 Silver Zone
Cleaned, deduplicated **Parquet** files partitioned by `education_level`. All 7 data quality issues resolved via PySpark.

### 🥇 Gold Zone
Aggregated business metrics partitioned by dimension (`device`, `country`, `education`, `satisfaction`). Directly consumed by Power BI.

---

## 🛠️ Tech Stack

| Service | Role |
|---------|------|
| **Azure Data Lake Storage Gen2** | Hierarchical cloud storage — raw/silver/gold containers |
| **Azure Data Factory** | Pipeline orchestration & sequential notebook triggering |
| **Azure Databricks + Apache Spark** | Distributed PySpark ETL — cleaning, deduplication, feature engineering |
| **Azure Synapse Analytics** | Serverless SQL querying via `OPENROWSET` on Parquet |
| **Microsoft Power BI** | Interactive dashboards & KPI visualization |

---

## 🧹 Data Quality Issues Resolved

| Issue Type | Scale | Fix Applied |
|------------|-------|-------------|
| Missing Values | ~219 nulls | Median imputation; `'Unknown'` for categorical |
| Duplicate Rows | 17 rows | `dropDuplicates()` — one canonical record per user |
| Inconsistent Labels | ~290 rows | PySpark `when()` standardisation to canonical forms |
| Outliers | ~27 rows | Domain-bound filters (age 10–100, hours 0–168, quiz 0–100) |
| Whitespace / Format | ~40 rows | `F.trim()` + `F.lower()` across all string columns |
| Wrong Data Types | All cols | Explicit casting to `IntegerType` / `DoubleType` |
| Label Case Mixing | ~50 rows | Lowercase comparison before matching (MALE/Male/male) |

---

## 📐 Feature Engineering

Three new columns created in the Silver layer:

- `engagement_score` — composite metric from login frequency and study hours
- `satisfaction_group` — Low / Medium / High bucketing of satisfaction rating
- `completed_binary` — standardised boolean from inconsistent completion flags

---

## 📊 Power BI Dashboards

### Executive Performance Dashboard
High-level summary KPIs and overall learner performance trends:
- Total users, avg quiz score, avg satisfaction, completion rate
- Completion rate by country (Australia leads)
- Device distribution (equal split: Desktop / Mobile / Tablet)
- Quiz score & completion rate by education level
- Performance trends by study band (hours per week)

### Behavioural & Advanced Analytics Dashboard
Deeper engagement and demographic analysis:
- Login frequency vs quiz score scatter (positive correlation at high engagement)
- Completion rate by satisfaction group (Low: 47.9%, Medium: 46.8%, High: 45.6%)
- Gender distribution breakdown
- Country-level satisfaction treemap (India avg 3.0, USA 3.11, UK 3.1)
- Age band × device satisfaction matrix

---

## 📁 Repository Structure

```
online-learning-azure-pipeline/
│
├── notebooks/
│   ├── 01_bronze_ingest.ipynb       # Schema standardisation, CSV → Parquet
│   ├── 02_silver_clean.ipynb        # All 7 cleaning operations + feature engineering
│   └── 03_gold_aggregate.ipynb      # Business-level aggregations by dimension
│
├── data/
│   ├── raw/
│   │   └── online_learning_dirty.csv
│   ├── silver/
│   │   └── online_learning_user_behavior.csv
│   └── gold/
│       ├── by_country.csv
│       ├── by_device.csv
│       ├── by_education.csv
│       ├── by_satisfaction.csv
│       ├── by_study_band.csv
│       ├── age_device_heatmap.csv
│       ├── gender_completion.csv
│       ├── login_vs_score.csv
│       └── silver_overview.csv
│
├── powerbi/
│   └── OnlineLearningAnalytics.pbix
│
├── report/
│   └── ProjectFile.pdf
│
└── README.md
```

---

## 🚀 How to Run

### Prerequisites
- Azure subscription (free tier or student credits work)
- Azure Databricks workspace
- Azure Data Lake Storage Gen2 account
- Azure Synapse Analytics workspace
- Power BI Desktop

### Steps

1. **Upload raw data** to ADLS Gen2 `raw/` container
2. **Import notebooks** into Azure Databricks
3. **Configure ADF pipeline** to trigger notebooks sequentially:
   - `01_bronze_ingest` → `02_silver_clean` → `03_gold_aggregate`
4. **Run Synapse SQL queries** against the Gold Parquet files
5. **Open Power BI** and connect to Synapse query outputs
6. **Refresh dashboards** to see live insights

---

## 🔭 Future Roadmap

| Phase | Enhancement | Azure Tool |
|-------|-------------|------------|
| Short-term | Real-Time Streaming | Azure Event Hub + Stream Analytics |
| Short-term | Automated Orchestration | ADF Triggers (scheduled / event-driven) |
| Mid-term | ML Completion Prediction | Azure Machine Learning (AutoML) |
| Mid-term | Predictive Dashboards | Power BI + Azure ML integration |
| Long-term | Anomaly Detection | Azure Anomaly Detector API |
| Long-term | Multi-Source Scaling | ADLS Gen2 + ADF Connectors |

---

## 👥 Team

| Name | Roll No. |
|------|----------|
| **Harshit Parpe** | 23/IT/064 |
| **Harsh Gautam Jha** | 23/IT/061 |

**Guide:** Prof. Khushbu Gupta

---

## 📄 License

This project is submitted as an academic cloud data engineering project. Dataset used for educational purposes only.

---

> *"Transforming raw, inconsistent data into actionable business insights through a scalable, end-to-end Medallion Architecture pipeline."*
