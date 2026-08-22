# lakehouse_medallion_architecture
Data Lakehouse on Databricks using the Medallion Architecture (Bronze/Silver/Gold), Unity Catalog, and orchestrated Jobs with bike company ERP/CRM data.
# Bike Data Lakehouse

A production-style **Data Lakehouse** built on **Databricks** using the **Medallion Architecture** (Bronze → Silver → Gold). This project takes raw operational data from a fictional bike company's ERP and CRM systems and transforms it end-to-end into clean, trusted, analytics-ready datasets — modeled as a star schema and fully automated with orchestrated jobs.

---

## Project Overview

Modern data platforms don't just move data — they progressively refine it. This project simulates that real-world workflow by building a Lakehouse from the ground up:

- **Bronze Layer** — raw ingestion of source CSV files with zero transformation, preserving a full history of the source system as-is.
- **Silver Layer** — cleansed, validated, and standardized data: deduplicated, type-corrected, key-standardized, and business-ready for joining.
- **Gold Layer** — a dimensional model (star schema) of fact and dimension tables purpose-built for BI, reporting, and analytics.

The entire pipeline is orchestrated and scheduled as a Databricks Job, so data flows automatically from raw source to business-ready output.

---

## Architecture

```
Source Systems (ERP + CRM CSVs)
        │
        ▼
   BRONZE  →  Raw ingestion, no transformations, source-system prefixed tables
        │
        ▼
   SILVER  →  Cleaned, deduplicated, standardized, validated
        │
        ▼
   GOLD    →  Star schema (fact_sales, dim_customers, dim_products)
        │
        ▼
   Analytics & Reporting
```

All layers live in **Unity Catalog** as separate schemas (`bronze`, `silver`, `gold`), with raw files staged in a dedicated Bronze volume.

---

## Tech Stack

- **Databricks** (Notebooks, Jobs & Pipelines, Unity Catalog)
- **PySpark & Spark SQL**
- **Delta Lake**
- **Git / GitHub** (version-controlled Databricks Git Folder integration)
- **draw.io** (architecture & data model diagrams)

---

## Repository Structure

```
├── bronze/
│   └── bronze_ingestion.ipynb
├── silver/
│   ├── crm/
│   │   ├── silver_crm_cust_info.ipynb
│   │   └── ...
│   └── erp/
│       ├── silver_erp_<table>.ipynb
│       └── ...
├── gold/
│   ├── gold_dim_customers.ipynb
│   ├── gold_dim_products.ipynb
│   └── gold_fact_sales.ipynb
├── orchestration/
│   ├── silver_orchestration.ipynb
│   └── gold_orchestration.ipynb
└── docs/
    ├── architecture_diagram.png
    └── data_model.png
```

---

## Data Flow & Layers

### Bronze — Raw Ingestion
Six source CSV files (ERP + CRM) are ingested as-is into Delta tables, prefixed by source system (`erp_`, `crm_`) for traceability. No cleaning or business logic is applied here — this layer is the system of record for raw history.

### Silver — Cleaned & Standardized
Each Bronze table is analyzed for data quality issues — duplicates, inconsistent formatting, invalid dates, non-standardized keys — and resolved through incremental, well-documented transformations. Output tables are given clear, analyst-friendly names and structures.

### Gold — Business-Ready Star Schema
Silver tables are joined and modeled into a dimensional schema (`fact_sales`, `dim_customers`, `dim_products`) optimized for BI tools and analytical queries. Gold tables include Unity Catalog metadata (descriptions, primary/foreign keys) to support self-service analytics.

---

## Orchestration & Automation

The pipeline is fully automated via a Databricks Job (`loading_bike_data_lakehouse`) with three sequential tasks:

1. **Bronze** — raw ingestion notebook
2. **Silver** — orchestration notebook triggering all Silver transformations
3. **Gold** — orchestration notebook triggering all Gold model builds

The job is scheduled to run on a recurring basis, with monitoring during the initial rollout period to validate reliability before reducing oversight.

---

## Getting Started

1. Clone this repository into a Databricks Git Folder (Workspace → Create → Git Folder).
2. Create the `bronze`, `silver`, and `gold` schemas in Unity Catalog.
3. Create a volume (`raw_sources`) inside the `bronze` schema and upload the source CSV files.
4. Run the Bronze notebook to ingest raw data.
5. Run the Silver notebooks (or the Silver orchestration notebook) to clean and standardize the data.
6. Run the Gold notebooks (or the Gold orchestration notebook) to build the star schema.
7. Configure and schedule the Databricks Job to automate the full pipeline.

---

## Roadmap / Future Enhancements

- Automated data quality checks (row counts, null checks, business rule validation)
- Reusable transformation functions and config-driven pipelines
- Additional data sources (APIs, streaming/Kafka, operational databases)
- CI/CD for automated testing and environment promotion
- Access control, row-level security, and data masking
- Pipeline monitoring, alerting, and observability
- Incremental/streaming ingestion with CDC and MERGE patterns

---

## Acknowledgments

This project was built as part of a guided Databricks/Lakehouse bootcamp curriculum. Credit to the original course design and reference materials from [DataWithBaraa](https://github.com/DataWithBaraa/databricks_bootcamp_2026).

---

## License

This project is licensed under the [MIT License](LICENSE) — free to use, modify, and share for educational and portfolio purposes.
