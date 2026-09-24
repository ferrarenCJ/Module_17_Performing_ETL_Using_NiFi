# Video 17.3 – NiFi: An Apache ETL Tool

**Duration:** 04:10

---

# Summary

Apache NiFi is an open-source data pipeline and transformation platform developed by the Apache Software Foundation.

NiFi simplifies the movement, transformation, tracking, and management of data between systems through a visual drag-and-drop interface.

The primary purpose of NiFi is to automate ETL processes and facilitate reliable data movement across diverse systems.

---

# What is Apache NiFi?

Apache NiFi is a:

- Data Pipeline Tool
- Data Integration Tool
- Data Transformation Tool
- ETL Platform

NiFi allows organizations to:

- Ingest data
- Transform data
- Route data
- Monitor data flows
- Automate workflows

without writing large amounts of custom code.

---

# Key Features of NiFi

## Visual Workflow Design

NiFi allows users to create data pipelines using a graphical interface.

Benefits:

- Easy to understand
- Easy to maintain
- Rapid development

---

## Data Provenance

NiFi tracks the movement of every piece of data.

Benefits:

- Auditing
- Troubleshooting
- Compliance
- Lineage Tracking

---

## Real-Time Data Movement

Supports:

- Batch Processing
- Streaming Data
- Near Real-Time Processing

---

## Scalability

Designed to process:

- Small datasets
- Large enterprise workloads
- Distributed environments

---

# Core NiFi Components

Three components form the foundation of NiFi.

## 1. FlowFiles

FlowFiles are the fundamental data objects moving through NiFi.

A FlowFile contains:

### Content

The actual data.

Examples:

- CSV data
- JSON data
- XML data
- Database records

### Attributes

Metadata describing the data.

Examples:

- Filename
- Source
- Timestamp
- Record Count

---

### FlowFile Example

```text
FlowFile
├── Content
│     sales.csv
│
└── Attributes
      filename=sales.csv
      source=mysql
      date=2026-09-24