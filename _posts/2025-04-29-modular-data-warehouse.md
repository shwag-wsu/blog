---
layout: post
title: "Modular Data Warehouse Reference Architecture"
categories: misc
---


## 🌐 Cloud-Agnostic Modern Data Warehouse Architecture (ELT)

This architecture outlines a flexible, scalable ELT (Extract, Load, Transform) approach that supports multiple cloud platforms — AWS, Azure, and Google Cloud. It separates data ingestion, storage, transformation, modeling, governance, and analytics, while promoting modularity and vendor neutrality.



![ELT](https://shwag-wsu.github.io/blog/elt_lib.png)

### 🔧 Modular Data Warehouse Reference Architecture

### 1. Sources
- Internal DBs: MySQL, PostgreSQL
- File-based: Google Sheets, Excel, CSVs
- SaaS: Salesforce, HubSpot

### 2. Data Ingestion
Data is ingested into the pipeline using both batch and streaming mechanisms:

<b>Batch Processing:</b> Regular scheduled jobs to pull from sources.

<b>Pub/Sub or Event Streams:</b> For real-time data (e.g., user activity, app logs).

Key Considerations:

Data is validated at ingestion for schema correctness, null values, and basic formatting.

Real-time and batch pipelines can coexist based on latency needs.

<b>Cloud Options</b>

Component             | AWS                   | Azure                    | GCP
--------------------- | --------------------- | ------------------------ | ---------------------
Batch Process         | AWS Glue              | Azure Data Factory       | Cloud Dataflow
Pub/Sub               | Amazon Kinesis        | Event Grid / Service Bus | Google Pub/Sub
lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit
lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit

### 3. Data Lake

Component             | AWS                   | Azure                    | GCP
--------------------- | --------------------- | ------------------------ | ---------------------
Storage               | S3                    | Azure Blob               | Google Cloud Storage
Database              | Snowflake             | Event Grid / Service Bus | Google Pub/Sub
