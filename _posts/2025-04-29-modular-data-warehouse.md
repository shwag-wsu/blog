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

Component	AWS	Azure	GCP
Batch Process	AWS Glue	Azure Data Factory	Cloud Dataflow
Pub/Sub	Amazon Kinesis	Event Grid / Service Bus	Google Pub/Sub



Component             | AWS                   | Azure                    | GCP
--------------------- | --------------------- | ------------------------ | ---------------------
Batch Process         | AWS Glue              | Azure Data Factory       | Cloud Dataflow
Pub/Sub               | Amazon Kinesis        | Event Grid / Service Bus | Google Pub/Sub
lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit
lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit
