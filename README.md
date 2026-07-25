
# 🛒 DataForge Commerce Platform

> **Production-inspired End-to-End Commerce Data Engineering Platform built using Apache Airflow, Databricks, Delta Lake, dbt, PostgreSQL, and Change Data Capture (CDC).**

---

# 📌 Project Overview

Modern commerce organizations process large volumes of transactional data generated from operational systems. Building a reliable analytics platform requires more than simply moving data from one system to another.

This project demonstrates how to build a **production-style modern data engineering platform** that supports incremental data ingestion, Change Data Capture (CDC), data transformation, data quality validation, dimensional modeling, and workflow orchestration.

The project simulates a retail commerce data platform using Walmart-style transactional datasets.

The platform implements a complete data lifecycle:

* 🗄️ Retail transactional data is stored in a PostgreSQL-compatible source database.
* 📂 CSV datasets are loaded into the source database using the project's data ingestion workflow.
* 🔄 Change Data Capture (CDC) is used to identify and propagate source data changes.
* 🥉 Incremental data is ingested into the Databricks Bronze layer.
* 🥈 Data is transformed into a consolidated **One Big Table (OBT)** for downstream processing.
* 🧪 Data quality checks validate transformed data before analytical modeling.
* 🥇 dbt is used to build analytics-ready dimensional models.
* 📊 The Gold layer implements a **Star Schema** with Fact and Dimension tables.
* 🔁 Slowly Changing Dimensions (SCD) are implemented to maintain historical business information.
* ⚙️ Apache Airflow orchestrates and coordinates the end-to-end data workflows.

The project follows a **Medallion Architecture** and demonstrates modern data engineering concepts such as CDC-based incremental ingestion, metadata-driven processing, Delta Lake, dbt transformations, data quality, SCD, and Star Schema modeling.

---

# 🚀 Project Highlights

- 🔄 Change Data Capture (CDC)
- ⚡ Incremental Data Ingestion
- ⚙️ Apache Airflow Orchestration
- 🧱 Databricks Lakehouse Architecture
- 🥉 Bronze / Silver / Gold Medallion Architecture
- 🗄️ PostgreSQL-Compatible Source Database
- 🧊 Delta Lake Tables
- 📊 One Big Table (OBT)
- 🧪 Data Quality Checks
- 🔧 dbt Transformations
- 📐 Star Schema Modeling
- 🕒 Slowly Changing Dimensions (SCD)
- 🧩 Metadata-Driven Processing
- 📈 Analytics-Ready Data Models

---

# 🏗️ Solution Architecture

## Overall Solution Architecture

![Architecture](images/architecture.png)

The platform follows a modern data engineering architecture where source data is incrementally captured and processed through multiple layers before being transformed into analytics-ready dimensional models.

---

# 🛠️ Tech Stack

<p align="center">
  <img src="https://go-skill-icons.vercel.app/api/icons?i=python,postgresql,airflow,databricks,dbt" />
</p>

<p align="center">
  PostgreSQL • Apache Airflow • Databricks • Delta Lake • dbt • SQL • Python • CDC • SCD • Star Schema
</p>

---

# 📂 Project Structure

```text
dataforge-commerce-platform
│
├── airflow
│   ├── dags
│   │   └── ...
│   └── ...
│
├── databricks
│   ├── bronze
│   │   └── ...
│   ├── silver
│   │   └── ...
│   └── gold
│       └── ...
│
├── dbt
│   ├── models
│   │   ├── staging
│   │   ├── silver
│   │   └── gold
│   │
│   ├── macros
│   ├── tests
│   ├── snapshots
│   ├── seeds
│   └── dbt_project.yml
│
├── data
│   └── walmart_dataset
│       └── data
│
├── ddl
│   └── walmart_schema.sql
│
├── images
│   ├── architecture.png
│   ├── bronze_layer.png
│   ├── silver_layer.png
│   ├── gold_layer.png
│   ├── airflow_dag.png
│   ├── databricks_pipeline.png
│   └── dbt_lineage.png
│
├── requirements.txt
├── docker-compose.yml
├── .gitignore
└── README.md
````

---

# 🔄 End-to-End Data Pipeline

The complete pipeline follows the flow:

```text
CSV Dataset
     │
     ▼
PostgreSQL-Compatible Source Database
     │
     │ CDC
     ▼
Databricks Bronze Layer
     │
     ▼
Incremental Processing
     │
     ▼
Silver Layer
     │
     ▼
One Big Table (OBT)
     │
     ▼
Data Quality Checks
     │
     ▼
dbt Transformations
     │
     ▼
Gold Layer
     │
     ▼
Star Schema
     │
     ├── Fact Tables
     │
     └── Dimension Tables
```

Apache Airflow is used to orchestrate and coordinate the workflow across the data platform.

---

# 1️⃣ Source Data & Database Layer

The project uses a **PostgreSQL-compatible source database** to simulate an operational retail commerce system.

The source database contains multiple retail datasets representing transactional and business entities.

The database schema is created using the provided DDL script:

```text
ddl/walmart_schema.sql
```

The schema defines the required source tables and their relationships.

The datasets are maintained as CSV files inside the project dataset directory.

```text
walmart_dataset/
└── data/
    ├── ...
    ├── ...
    └── ...
```

The CSV data is loaded into the corresponding source tables before the ingestion pipeline begins.

---

# 2️⃣ Data Ingestion into Source Database

The raw CSV datasets are loaded into the PostgreSQL-compatible source database.

The ingestion process maps each CSV dataset to its corresponding source table.

This creates a realistic operational source layer from which downstream data pipelines can consume data.

```text
CSV Files
    │
    ▼
Source Database
    │
    ├── Table 1
    ├── Table 2
    ├── Table 3
    └── ...
```

This approach allows the project to simulate a real-world scenario where data originates from operational database systems rather than directly from static files.

---

# 3️⃣ Change Data Capture (CDC)

To support incremental processing, the pipeline uses **Change Data Capture (CDC)** to identify changes occurring in the source data.

Instead of repeatedly processing the entire dataset, the ingestion layer captures newly inserted or changed records.

```text
Source Database
       │
       │ CDC
       ▼
Changed / New Records
       │
       ▼
Databricks Bronze Layer
```

This approach reduces unnecessary data processing and enables the platform to support incremental data ingestion patterns.

---

# 🥉 Bronze Layer

The Bronze layer is the raw ingestion layer of the Databricks Lakehouse.

Data is incrementally loaded from the PostgreSQL-compatible source database into Databricks Bronze tables.

The Bronze layer is responsible for preserving source-level data with minimal transformations.

### Bronze Layer Responsibilities

* Capture source data changes
* Process incremental records
* Maintain raw data history
* Store data using Delta Lake
* Provide a reliable foundation for downstream transformations

The Bronze layer acts as the initial landing zone for the data platform.

![Bronze Layer](images/bronze_layer.png)

---

# 🥈 Silver Layer

The Silver layer is responsible for transforming and preparing the ingested data for analytical processing.

Data from multiple Bronze tables is combined and transformed to create a consolidated **One Big Table (OBT)**.

The OBT provides a unified representation of the business data and simplifies downstream analytical transformations.

```text
Bronze Tables
     │
     ├── Table A
     ├── Table B
     ├── Table C
     └── Table D
           │
           ▼
      Transformations
           │
           ▼
    One Big Table (OBT)
```

The Silver layer performs operations such as:

* Data cleansing
* Data type standardization
* Joins
* Business transformations
* Incremental processing
* Record enrichment

The resulting OBT acts as the foundation for downstream dimensional modeling.

![Silver Layer](images/silver_layer.png)

---

# 🧪 Data Quality Checks

Data quality validation is performed before the data moves into the final analytical models.

The validation layer helps ensure that transformed datasets meet expected quality requirements.

Typical checks include:

* Null value validation
* Duplicate record detection
* Primary key validation
* Referential integrity
* Accepted value checks
* Data consistency validation

```text
Silver / OBT
     │
     ▼
Data Quality Checks
     │
     ├── PASS ──► Gold Models
     │
     └── FAIL ──► Pipeline Failure / Investigation
```

This ensures that only validated data is promoted to downstream analytical models.

---

# 🔧 dbt Transformation Layer

The project uses **dbt** to implement modular SQL-based transformations and analytical data models.

dbt provides a structured transformation framework that allows the project to manage:

* Staging models
* Intermediate transformations
* Incremental models
* Dimensional models
* Data tests
* Macros
* Model dependencies

The dbt DAG provides visibility into the relationships between different transformation models.

```text
Source / Bronze
       │
       ▼
Staging Models
       │
       ▼
Intermediate Models
       │
       ▼
Dimensional Models
       │
       ▼
Analytics Layer
```

Reusable dbt macros are used to simplify and standardize schema and transformation logic across the project.

---

# 🥇 Gold Layer

The Gold layer contains business-ready analytical models designed for reporting and downstream analytics.

The final data model follows a **Star Schema** architecture consisting of:

### Fact Tables

Fact tables contain measurable business events and transactional information.

Examples include:

* Orders
* Sales
* Transactions
* Order Items

### Dimension Tables

Dimension tables provide descriptive business context.

Examples include:

* Customers
* Products
* Stores
* Locations
* Dates

The final model is optimized for:

* Business Intelligence
* Analytical SQL queries
* Reporting
* Data visualization
* Aggregation
* Downstream analytics

![Gold Layer](images/gold_layer.png)

---

# 🕒 Slowly Changing Dimensions (SCD)

The platform demonstrates Slowly Changing Dimension patterns to manage changes in dimensional data over time.

### SCD Type 1

Used when historical changes do not need to be preserved.

```text
Old Value
    │
    ▼
Updated Value
```

The existing record is overwritten with the latest value.

### SCD Type 2

Used when historical changes must be preserved.

```text
Customer
    │
    ├── Version 1
    │
    ├── Version 2
    │
    └── Current Version
```

This approach allows analytical users to understand how dimensional attributes changed over time.

---

# ⭐ Star Schema

The final analytical model follows a Star Schema design.

```text
                 dim_customer
                       │
                       │
dim_product ──── fact_orders ──── dim_store
                       │
                       │
                  dim_date
```

The Star Schema provides:

* Simplified analytical queries
* Clear business relationships
* Efficient aggregations
* BI-friendly data structures
* Separation of facts and dimensions

---

# ⚙️ Apache Airflow Orchestration

Apache Airflow is used as the orchestration layer for the data platform.

The DAG coordinates the execution of ingestion, transformation, validation, and downstream processing tasks.

A simplified workflow can be represented as:

```text
Start
  │
  ▼
Source Data Availability
  │
  ▼
Incremental / CDC Ingestion
  │
  ▼
Bronze Processing
  │
  ▼
Silver / OBT Processing
  │
  ▼
Data Quality Checks
  │
  ▼
dbt Transformations
  │
  ▼
Gold Data Models
  │
  ▼
End
```

Airflow provides:

* Workflow orchestration
* Task dependencies
* Scheduling
* Monitoring
* Retry handling
* Pipeline execution management

![Airflow DAG](images/airflow_dag.png)

---

# 🔁 Complete Data Flow

The complete platform can be summarized as:

```text
                    ┌──────────────────────┐
                    │   CSV Retail Data    │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ PostgreSQL-Compatible│
                    │    Source Database   │
                    └──────────┬───────────┘
                               │
                               │ CDC
                               ▼
                    ┌──────────────────────┐
                    │ Databricks Bronze    │
                    │     Delta Tables     │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │    Silver Layer      │
                    │  Transformations     │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │   One Big Table      │
                    │        (OBT)         │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │   Quality Checks     │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │    dbt Models        │
                    │ Incremental + SCD    │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │    Gold Layer        │
                    │    Star Schema       │
                    └──────────────────────┘

                 Apache Airflow
          Orchestrates the overall workflow
```

---

# 📊 Project Architecture

The platform follows the **Medallion Architecture**:

```text
                 DATAFORGE COMMERCE PLATFORM

┌─────────────────────────────────────────────────────────┐
│                     SOURCE LAYER                        │
│                                                         │
│          PostgreSQL-Compatible Source Database          │
│                                                         │
└───────────────────────────┬─────────────────────────────┘
                            │
                            │ CDC / Incremental Ingestion
                            ▼
┌─────────────────────────────────────────────────────────┐
│                    BRONZE LAYER                         │
│                                                         │
│             Raw Delta Tables in Databricks              │
│                                                         │
└───────────────────────────┬─────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│                    SILVER LAYER                         │
│                                                         │
│          Transformations + One Big Table (OBT)          │
│                                                         │
└───────────────────────────┬─────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│                 DATA QUALITY LAYER                      │
│                                                         │
│        Validation + Data Tests + Integrity Checks       │
│                                                         │
└───────────────────────────┬─────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│                     GOLD LAYER                           │
│                                                         │
│       dbt Models + Star Schema + SCD Dimensions         │
│                                                         │
└─────────────────────────────────────────────────────────┘

                    ▲
                    │
             Apache Airflow
             Orchestration
```

---

# 🎯 Key Engineering Concepts Demonstrated

This project demonstrates practical implementation of several modern data engineering concepts:

### Data Ingestion

* Incremental data ingestion
* Change Data Capture (CDC)
* Source-to-Lakehouse integration

### Data Architecture

* Medallion Architecture
* Lakehouse architecture
* Bronze / Silver / Gold layers

### Data Transformation

* SQL transformations
* dbt models
* Incremental models
* Reusable macros

### Data Modeling

* One Big Table (OBT)
* Star Schema
* Fact and Dimension tables
* Slowly Changing Dimensions

### Data Quality

* Data validation
* dbt tests
* Data integrity checks

### Orchestration

* Apache Airflow
* DAG-based workflows
* Task dependencies
* Pipeline scheduling

---

# 📈 Future Improvements

Potential future enhancements include:

* Add real-time streaming ingestion using Kafka or Event Hubs
* Implement automated CI/CD for dbt and Airflow
* Add Great Expectations for advanced data quality validation
* Implement data observability and monitoring
* Add Unity Catalog for centralized governance
* Add Power BI dashboards for business analytics
* Implement automated alerting for pipeline failures

---

# 👨‍💻 Author

**Avinash Kamble**

Aspiring Azure Data Engineer | Data Engineering Enthusiast

---

⭐ If you found this project useful, feel free to explore the repository and connect with me.

```

