# Video 17.1 – Introduction and Information

**Duration:** 01:52

---

# Module Overview

Module 17 introduces Apache NiFi, an open-source data pipeline and transformation tool developed by the Apache Software Foundation.

The module focuses on building Extract, Transform, and Load (ETL) pipelines and integrating data across different platforms and database technologies.

Students will learn:

- ETL concepts and use cases
- NiFi architecture
- FlowFiles
- Processors
- Connectors
- Database integration
- Container-based deployments
- Building complete ETL pipelines

---

# Why This Module Matters

Up to this point in the program, the focus has been on:

- Databases
- SQL
- Python
- Data Engineering Concepts
- Change Data Capture (CDC)
- Kafka and Event Streaming
- Containerized Applications

Module 17 introduces a visual ETL orchestration platform that can automate data movement between systems.

Apache NiFi allows engineers to design, deploy, monitor, and manage data flows with minimal coding.

---

# Key Concepts Introduced

## ETL

### Extract

Retrieve data from a source system.

Examples:

- Excel Files
- CSV Files
- Databases
- APIs

### Transform

Modify data before loading.

Examples:

- Data Cleansing
- Data Formatting
- Filtering
- Aggregation

### Load

Move processed data into a destination.

Examples:

- MySQL
- MongoDB
- Redis
- Cassandra

---

## Apache NiFi

Apache NiFi is:

- Open Source
- Web-Based
- Flow-Oriented
- Drag-and-Drop ETL Tool

Primary purpose:

- Data ingestion
- Data movement
- Data transformation
- Workflow automation

---

## Containers

The module continues using containerized technology.

Containers will be used to run:

- MySQL
- MongoDB
- Redis
- Cassandra
- Supporting tools

Benefits:

- Portability
- Consistency
- Easy setup
- Isolation

---

## Drivers

Database drivers allow NiFi to connect to target systems.

Examples:

- MySQL JDBC Driver
- MongoDB Connector
- Cassandra Connector

---

## FlowFiles

FlowFiles are the fundamental unit of data inside NiFi.

A FlowFile contains:

- Content
- Attributes
- Metadata

FlowFiles move through a pipeline from source to destination.

---

## Processors

Processors perform work on FlowFiles.

Examples:

- Read File
- Transform Data
- Route Data
- Insert into Database

Processors are connected together to create workflows.

---

## Connectors

Connectors create relationships between processors and systems.

Purpose:

- Transport data
- Enable communication
- Route flowfiles

---

# Technologies Used in Modules 17-19

Students should ensure the required tools are installed before proceeding.

Expected technologies include:

- Apache NiFi
- Docker
- MySQL
- MongoDB
- Redis
- Cassandra
- Python
- SQL

---

# Connection to Previous Modules

Module 17 builds upon concepts learned earlier:

## Databases

Understanding relational and NoSQL databases.

## Containers

Running applications using Docker.

## Programming Languages

Using Python and SQL for data engineering tasks.

## CDC

Understanding how data changes move through systems.

## Project 16.1

Transit Data Application using Boston transit data.

Knowledge gained from Project 16.1 will support ETL pipeline development.

---

# Program Learning Outcomes

## Outcome 1

Explain key data science and data engineering concepts.

## Outcome 2

Develop and analyze databases using:

- SQL
- Python
- Data Engineering Tools

---

# Module Learning Outcomes

By the end of this module, students should be able to:

1. Identify use cases of ETL in data engineering.
2. Identify basic elements of NiFi.
3. Describe pros and cons of Apache ETL tools.
4. Use NiFi to create an ETL pipeline.

---

# Major Activities

## Discussions

- Discussion 17.1
- Discussion 17.2

## Coding Activities

- Activity 17.1: Connect NiFi to Excel
- Activity 17.2: Load Data to MySQL
- Activity 17.3: Create MongoDB Pipeline
- Activity 17.4: Create Redis Pipeline
- Activity 17.5: Create Cassandra Pipeline

## Assignment

- Assignment 17.1 Performing ETL Using NiFi

---

# Exam / Knowledge Check Notes

Remember:

- NiFi is an Apache ETL tool.
- FlowFiles are NiFi's basic data units.
- Processors perform operations on data.
- Connectors move data between components.
- Containers are used to deploy database services.
- ETL = Extract + Transform + Load.
- NiFi supports multiple database targets.

---

# Personal Notes

Module 17 marks the transition from CDC and streaming-focused workflows into visual ETL orchestration.

Key focus areas:

- Apache NiFi Architecture
- ETL Pipeline Design
- Database Connectivity
- MySQL Integration
- MongoDB Integration
- Redis Integration
- Cassandra Integration

This module appears highly hands-on and serves as the foundation for Modules 18–19.