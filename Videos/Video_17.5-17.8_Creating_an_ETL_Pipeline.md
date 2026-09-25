# Videos 17.5–17.8: Creating an ETL Pipeline

## Overview

In this section, Apache NiFi is used to build a complete ETL pipeline.

The implementation consists of:

1. Creating a Docker network
2. Deploying MySQL and NiFi containers
3. Initializing a source database
4. Installing JDBC drivers
5. Configuring NiFi processors
6. Creating FlowFiles and connectors
7. Executing the ETL pipeline

---

# ETL Architecture

```text
Docker Network
│
├── MySQL Container
│       │
│       ▼
│   Source Database
│
└── NiFi Container
        │
        ▼
     ETL Pipeline
        │
        ▼
   Target Destination
```

---

# Video 17.5 – Installing NiFi in a Container

## Objective

Create Docker containers for:

- Apache NiFi
- MySQL

Both containers run inside the same Docker network.

---

## Why Use Docker?

Benefits:

- Isolation
- Portability
- Repeatability
- Simplified deployment

---

## Deployment Architecture

```text
Docker Host
│
├── MySQL Container
│
└── NiFi Container
```

---

## Docker Network

Both containers communicate through:

```text
Docker Network
```

This allows:

- Database connectivity
- Internal name resolution
- Secure communication

---

## Important Notes

The course demonstrates:

```text
Apache NiFi 1.13.2
```

Current versions may be newer.

Core concepts remain identical.

---

# Video 17.6 – Creating and Initializing a Database in a Container

## Objective

Create and initialize a MySQL database running inside Docker.

---

## Tools Used

### MySQL

Source database.

### MySQL Workbench

Administrative client used to:

- Connect
- Create schemas
- Execute SQL
- Validate data

---

## Database Initialization Process

### Step 1

Connect to MySQL container.

### Step 2

Create database.

Example:

```sql
CREATE DATABASE studentdb;
```

### Step 3

Create tables.

Example:

```sql
CREATE TABLE students (
    id INT,
    first_name VARCHAR(50),
    last_name VARCHAR(50)
);
```

### Step 4

Insert sample records.

Example:

```sql
INSERT INTO students
VALUES (1,'John','Smith');
```

### Step 5

Verify records.

```sql
SELECT *
FROM students;
```

---

## Purpose

The database serves as the source system for ETL extraction.

```text
MySQL
    ↓
Extract
    ↓
NiFi
```

---

# Video 17.7 – Installing a Driver in a Container

## Objective

Install the JDBC driver required for database connectivity.

---

## What is a JDBC Driver?

JDBC stands for:

```text
Java Database Connectivity
```

A JDBC driver allows NiFi to communicate with databases.

---

## Why Is It Needed?

Without a JDBC driver:

```text
NiFi ❌ Database
```

With a JDBC driver:

```text
NiFi ✅ Database
```

---

## MySQL Driver Used

Course version:

```text
MySQL Connector/J 8.0
```

Current releases may use newer versions.

---

## Driver Installation Process

### Download Driver

Example:

```text
mysql-connector-java.jar
```

### Copy Driver

Place driver inside NiFi environment.

### Configure Controller Service

Reference:

```text
DBCPConnectionPool
```

### Test Database Connectivity

Verify:

- Connection successful
- Driver loaded
- Database reachable

---

# JDBC Architecture

```text
NiFi
  │
  ▼
JDBC Driver
  │
  ▼
MySQL Database
```

---

# Video 17.8 – Using NiFi to Create an ETL Pipeline

## Objective

Build an end-to-end ETL workflow using:

- FlowFiles
- Processors
- Connectors

---

# NiFi Pipeline Overview

```text
MySQL
   │
   ▼
Generate FlowFile
   │
   ▼
Execute SQL
   │
   ▼
Transform Data
   │
   ▼
Load Destination
```

---

# Step 1 – Configure Database Connection

Create a Controller Service.

Example:

```text
DBCPConnectionPool
```

Configuration:

- Database URL
- Username
- Password
- JDBC Driver

---

# Step 2 – Create Processors

NiFi pipelines are built using processors.

Examples:

### GenerateFlowFile

Creates a FlowFile.

### ExecuteSQL

Extracts records from MySQL.

### ConvertRecord

Transforms records.

### PutDatabaseRecord

Loads data into destination database.

---

# Step 3 – Create Connectors

Connect processors together.

Example:

```text
GenerateFlowFile
       │
       ▼
ExecuteSQL
       │
       ▼
ConvertRecord
       │
       ▼
PutDatabaseRecord
```

---

# Step 4 – Execute Flow

When processors start:

```text
FlowFiles move through pipeline
```

Operations performed:

- Extraction
- Transformation
- Loading

---

# FlowFile Lifecycle

```text
Create
   ↓
Read
   ↓
Transform
   ↓
Route
   ↓
Store
```

---

# End-to-End ETL Process

## Extract

Retrieve data from MySQL.

Example:

```sql
SELECT *
FROM students;
```

---

## Transform

Possible operations:

- Filtering
- Formatting
- Validation
- Aggregation

---

## Load

Store processed data into destination system.

Examples:

- MySQL
- MongoDB
- Redis
- Cassandra

---

# Complete Architecture

```text
+----------------+
| MySQL Database |
+----------------+
         │
         ▼
+----------------+
| JDBC Driver    |
+----------------+
         │
         ▼
+----------------+
| Apache NiFi    |
+----------------+
         │
         ▼
+----------------+
| Destination    |
| Database       |
+----------------+
```

---

# Key Concepts Introduced

## Docker Network

Provides communication between containers.

---

## MySQL Container

Source database.

---

## JDBC Driver

Enables NiFi database connectivity.

---

## FlowFile

Basic unit of data in NiFi.

---

## Processor

Performs operations on FlowFiles.

Examples:

- Read
- Transform
- Route
- Load

---

## Connector

Moves FlowFiles between processors.

---

## Controller Service

Manages shared resources.

Most important example:

```text
DBCPConnectionPool
```

---

# Exam Notes

✅ NiFi and MySQL run in separate Docker containers

✅ Both containers communicate through a shared Docker network

✅ JDBC drivers are required for database connectivity

✅ DBCPConnectionPool manages database connections

✅ FlowFiles are the data objects processed by NiFi

✅ Processors perform ETL operations

✅ Connectors define the flow between processors

✅ ETL = Extract → Transform → Load

✅ NiFi can extract data directly from MySQL

✅ JDBC acts as the bridge between NiFi and the database

---

# Personal Notes

These four videos represent the first complete implementation of an enterprise ETL architecture in the program.

Most important concepts:

1. Docker Networking
2. MySQL Containers
3. JDBC Drivers
4. DBCPConnectionPool
5. FlowFiles
6. Processors
7. Connectors
8. End-to-End ETL Pipelines

Everything in the remaining coding activities builds on this architecture.