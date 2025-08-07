

# Data-Driven Telecom Decision Platform in Azure:

## Business Use Case

This project addresses a scenario for a technology company in the telecom domain.Following a new product launch in January 2022, the company observed a decline in user engagement and revenue growth.

The objective is to build an end-to-end data pipeline that ingests, processes, and analyzes key performance indicators (KPIs) from before and after the launch. The resulting analytics report will empower management to make data-driven decisions to improve user engagement, optimize product offerings, and enhance user acquisition.

## Architecture

This project implements a modern data platform on Microsoft Azure, featuring a multi-layered data lake architecture and integrating with GCP for source data.



## Tech Stack

* **Cloud Platforms**: Microsoft Azure, Google Cloud Platform (GCP) 
* **Data Ingestion & Orchestration**: Azure Data Factory (ADF) 
* **Data Processing**: Azure Databricks (PySpark) 
* **Data Storage**: Azure Data Lake Storage (ADLS) Gen2 
* **Data Governance**: Databricks Unity Catalog (UC), RBAC 
* **Secrets Management**: Azure Key Vault 
* **Alerting & Notifications**: Azure Logic Apps 
* **CI/CD & DevOps**: Azure DevOps (ADO) 

## Data Pipeline Overview

The pipeline follows a medallion architecture to progressively refine data from its raw state to an analysis-ready format.

1.  **Ingestion (GCP to Landing Zone)**
    * ADF pipelines are used to ingest source CSV files from a Google Cloud Storage bucket into the ADLS Gen2 `landing` container.
    * The pipeline framework is parameterized and includes checks for file availability, naming, and format.It supports both full and delta load patterns.

2.  **Transformation (Landing -> Raw -> Intermediate)**
    * Azure Databricks notebooks are triggered by ADF to process the data.
    * **Landing to Raw**: Data is cleaned, and derived columns (`load_id`, `last_inserted_dttm_azure`) are added. [cite_start]Bad records are handled, and duplicates are checked. After processing, source files are archived.
    * **Raw to Intermediate**: Business logic is applied using Databricks notebooks to merge and transform data into an intermediate format, preparing it for final aggregation.

3.  **Curated Layer & Analytics**
    * A final ADF pipeline orchestrates Databricks notebooks to apply transformations like joins and CTEs, loading the data into the `curated` zone. This layer serves as the single source of truth for BI and analytics.

4.  **Governance and Security**
    * **Unity Catalog**: A metastore is created in Databricks Unity Catalog to govern the data assets.Tables created in the Hive Metastore are synced to Unity Catalog.
    * **RBAC**: Role-Based Access Control is configured by creating user groups and assigning them specific permissions on schemas and tables within Unity Catalog to ensure data security.

## Key Features Demonstrated

* **End-to-End ETL Pipeline**: Implementation of a complete, automated data pipeline from ingestion to analytics.
* **Medallion Architecture**: Data is structured into Bronze (raw), Silver (intermediate), and Gold (curated) layers to ensure data quality and governance.
* **Hybrid Cloud Integration**: Ingestion of data from GCP into an Azure-native data platform.
* **Delta and Full Load Patterns**: The framework is designed to handle both incremental (delta) and complete (full) data loads efficiently.
* **Infrastructure as Code (IaC) Principles**: While not fully scripted with Terraform/ARM, the project follows a structured setup of resources, including VNet, subnets, and all necessary Azure services.
* **Dynamic & Parameterized Pipelines**: Azure Data Factory pipelines are heavily parameterized for reusability and maintainability.
* **Secrets Management**: Secure storage and retrieval of credentials and keys using Azure Key Vault, integrated with ADF and Databricks.
* **Automated Alerting**: Azure Logic Apps are configured to send automated email notifications for pipeline status (in-progress, success, error) and data quality issues (count mismatch).
* **CI/CD for DataOps**: The project plan includes creating and executing Azure DevOps release pipelines to deploy ADF and Databricks artifacts to a production environment.
