# Parish Household Census Pipeline


A simulated data engineering project built in Databricks, utilizing PySpark and Delta Lake. 

This project is a portfolio version of a real household census report developed for the pastor of an Orthodox parish. Sample data using names and households from the Bethesda title *The Elder Scrolls V: Skyrim* were used with simulated personal data to present the architecture, transformations, validation, and reporting functionality of the pipeline.

## Project Overview

The parish data arrives as an Excel workbook containing household and person-level information, as recorded by the pastor. The pipeline ingests, cleans, and validates the data, and proceeds to model and aggregate the metrics desired by the archdiocese. 

The project uses a **Medallion Architecture**:

**Bronze → Silver → Gold → Report**

- **Bronze:** Raw source ingestion and Delta persistence
- **Silver:** Cleaning, normalization, entity construction, and automated data-quality validation
- **Gold:** Census metrics and aggregate views
- **Report:** Human-readable census summary generated from Gold-layer outputs

The complete workflow is orchestrated from a Databricks notebook.

---

## Architecture

```text
Excel Source
     │
     ▼
┌─────────────┐
│   Bronze    │
│ Raw Ingest  │
└──────┬──────┘
       │
       ▼
┌───────────────────┐
│      Silver       │
│ Clean + Transform │
│ Entity Keys       │
│ QA Validation     │
└─────────┬─────────┘
          │
          ▼
┌───────────────────┐
│       Gold        │
│ Census Metrics    │
│ Aggregate Views   │
└─────────┬─────────┘
          │
          ▼
┌───────────────────┐
│      Report       │
│ Human-Readable    │
│ Census Summary    │
└───────────────────┘
```

---

## Technology Stack

- **Databricks**
- **Apache Spark / PySpark**
- **Delta Lake**
- **Python**
- **Pandas**
- **Excel / XLSX source data**
- **Databricks notebook orchestration**


---

## Data Pipeline

### Bronze Layer — Raw Ingestion

Ingests the pastor's data without transformation. 

### Silver Layer — Cleaning, Modeling, and QA

Carries out normalization and performs QA checks on the following:
- Null household keys
- Duplicate person keys
- Duplicate household keys
- Orphan household keys
- Person row count preservation
- Household row count preservation
- Future-dated DOBs


**QA result for the portfolio dataset:** `[INSERT RESULT / COUNTS]`



### Gold Layer — Census Metrics

Derives a demographics dimension from person data by calculating age, and creates a contributing-member view from tithing and volunteer data.

![Gold-layer demographics output](images/gold_demographics.png)

### Reporting

Aggregates demographics by requested age bands, total membership, and contributing membership. Contributing membership is defined at the household level: a household is contributing if at least one member is either tithing or volunteering. 

![Generated parish census report](images/census_report.png)

---

## Orchestration

The complete workflow can be executed from a single orchestration notebook:

```text
01_bronze_ingestion
        ↓
02_silver_transforms
        ↓
03_gold_marts
        ↓
04_report
```

The orchestrator executes each stage sequentially and records pipeline progress and failures through logging.

![Successful pipeline execution](images/orchestrator_run.png)

---

## Repository Structure

```text
parish_census_portfolio/
│
├── 00_orchestrator
├── 01_bronze_ingestion
├── 02_silver_transformation
├── 03_gold_aggregation
├── 04_census_report
│
├── sample_data/
│   └── [ANONYMIZED DATASET]
│
├── reports/
│   └── [SAMPLE REPORT]
│
└── README.md
```

*Update this tree to match the final repository structure.*

---

## Privacy and Anonymization

This repository is based on a real data product created for a parish. No original personally identifiable parishioner information is included in the portfolio project.

The public version uses simulated data while retaining the underlying engineering solution.

---

## Running the Project

### Requirements

- Databricks workspace
- Spark-compatible compute
- Source workbook matching the expected schema

### Execution

Run:

```text
00_orchestrator
```

The orchestrator executes the complete Bronze → Silver → Gold → Report workflow.

Individual notebooks can also be executed independently during development or debugging.

---

## Potential Extensions

The current implementation focuses on the requirements of the original census product, within the limitations of the Databricks free tier. Possible production extensions include:

- Incremental source ingestion
- Delta `MERGE`-based upserts
- Additional schema and business-rule validation
- Historical census snapshots
- Automated report delivery
- Databricks Workflows scheduling

---
