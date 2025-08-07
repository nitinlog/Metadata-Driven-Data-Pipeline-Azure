

# Azure End-to-End Data Engineering Project: Telecom Analytics

## Business Use Case

[cite_start]This project addresses a scenario for a technology company in the telecom domain[cite: 5, 7]. [cite_start]Following a new product launch in January 2022, the company observed a decline in user engagement and revenue growth[cite: 8].

The objective is to build an end-to-end data pipeline that ingests, processes, and analyzes key performance indicators (KPIs) from before and after the launch. [cite_start]The resulting analytics report will empower management to make data-driven decisions to improve user engagement, optimize product offerings, and enhance user acquisition[cite: 8, 9, 10].

## Architecture

This project implements a modern data platform on Microsoft Azure, featuring a multi-layered data lake architecture and integrating with GCP for source data.

*(You can embed your `Azure Project-2 Design.jpg` image here by placing it in your repository and adding the following line)*
`![Architecture Diagram](Azure Project-2 Design.jpg)`

## Tech Stack

* [cite_start]**Cloud Platforms**: Microsoft Azure, Google Cloud Platform (GCP) [cite: 2]
* [cite_start]**Data Ingestion & Orchestration**: Azure Data Factory (ADF) [cite: 2]
* [cite_start]**Data Processing**: Azure Databricks (PySpark) [cite: 2, 74]
* [cite_start]**Data Storage**: Azure Data Lake Storage (ADLS) Gen2 [cite: 33]
* [cite_start]**Data Governance**: Databricks Unity Catalog (UC), RBAC [cite: 2, 125, 135]
* [cite_start]**Secrets Management**: Azure Key Vault [cite: 34]
* [cite_start]**Alerting & Notifications**: Azure Logic Apps [cite: 35, 117]
* [cite_start]**CI/CD & DevOps**: Azure DevOps (ADO) [cite: 21]

## Data Pipeline Overview

The pipeline follows a medallion architecture to progressively refine data from its raw state to an analysis-ready format.

1.  **Ingestion (GCP to Landing Zone)**
    * [cite_start]ADF pipelines are used to ingest source CSV files from a Google Cloud Storage bucket into the ADLS Gen2 `landing` container[cite: 62, 68].
    * [cite_start]The pipeline framework is parameterized and includes checks for file availability, naming, and format[cite: 68, 70, 71, 72]. [cite_start]It supports both full and delta load patterns[cite: 2, 73, 80].

2.  **Transformation (Landing -> Raw -> Intermediate)**
    * [cite_start]Azure Databricks notebooks are triggered by ADF to process the data[cite: 74, 89].
    * [cite_start]**Landing to Raw**: Data is cleaned, and derived columns (`load_id`, `last_inserted_dttm_azure`) are added[cite: 86]. [cite_start]Bad records are handled, and duplicates are checked[cite: 85, 88]. [cite_start]After processing, source files are archived[cite: 97].
    * [cite_start]**Raw to Intermediate**: Business logic is applied using Databricks notebooks to merge and transform data into an intermediate format, preparing it for final aggregation[cite: 95, 99, 105, 107].

3.  **Curated Layer & Analytics**
    * [cite_start]A final ADF pipeline orchestrates Databricks notebooks to apply transformations like joins and CTEs, loading the data into the `curated` zone[cite: 111, 112, 114, 115]. This layer serves as the single source of truth for BI and analytics.

4.  **Governance and Security**
    * [cite_start]**Unity Catalog**: A metastore is created in Databricks Unity Catalog to govern the data assets[cite: 127, 128]. [cite_start]Tables created in the Hive Metastore are synced to Unity Catalog[cite: 134].
    * [cite_start]**RBAC**: Role-Based Access Control is configured by creating user groups and assigning them specific permissions on schemas and tables within Unity Catalog to ensure data security[cite: 135, 136, 140].

## Key Features Demonstrated

* **End-to-End ELT Pipeline**: Implementation of a complete, automated data pipeline from ingestion to analytics.
* **Medallion Architecture**: Data is structured into Bronze (raw), Silver (intermediate), and Gold (curated) layers to ensure data quality and governance.
* [cite_start]**Hybrid Cloud Integration**: Ingestion of data from GCP into an Azure-native data platform[cite: 60].
* [cite_start]**Delta and Full Load Patterns**: The framework is designed to handle both incremental (delta) and complete (full) data loads efficiently[cite: 2].
* [cite_start]**Infrastructure as Code (IaC) Principles**: While not fully scripted with Terraform/ARM, the project follows a structured setup of resources, including VNet, subnets, and all necessary Azure services[cite: 24, 28].
* [cite_start]**Dynamic & Parameterized Pipelines**: Azure Data Factory pipelines are heavily parameterized for reusability and maintainability[cite: 68].
* [cite_start]**Secrets Management**: Secure storage and retrieval of credentials and keys using Azure Key Vault, integrated with ADF and Databricks[cite: 34, 52, 56].
* [cite_start]**Automated Alerting**: Azure Logic Apps are configured to send automated email notifications for pipeline status (in-progress, success, error) and data quality issues (count mismatch)[cite: 117, 118, 119, 120, 121, 122].
* [cite_start]**CI/CD for DataOps**: The project plan includes creating and executing Azure DevOps release pipelines to deploy ADF and Databricks artifacts to a production environment[cite: 144, 145].
