# Video 17.9: Recap - Performing ETL Using NiFi

## Overview

In this recap video, Dr. Sanchez summarizes the ETL concepts covered throughout Module 17 and reviews how Apache NiFi can be used to build end-to-end ETL pipelines. The module demonstrated how NiFi extracts data from multiple sources, transforms the data into the required format, and loads it into target systems.

The video recommends experimenting with NiFi by creating additional pipelines and connecting new data sources to further develop ETL skills.

---

# ETL Overview

ETL stands for:

```text
Extract
Transform
Load
```

General data flow:

```text
Source System
      ↓
Extract
      ↓
Transform
      ↓
Load
      ↓
Target System
```

Apache NiFi provides a low-code graphical interface that simplifies the design and management of ETL workflows.

---

# NiFi Components Used Throughout Module 17

## Extract Processors

Used to read data from source systems.

Examples:

```text
QueryCassandra
QueryMongo
```

Purpose:

- Retrieve records from databases
- Convert source data into FlowFiles
- Send data into the ETL pipeline

---

## Transformation Processors

Used to reshape or manipulate incoming data.

Examples:

```text
SplitJSON
ConvertJSONToSQL
```

Purpose:

- Break large JSON documents into individual records
- Convert JSON data into SQL statements suitable for relational databases

---

## Load Processors

Used to write transformed data to target systems.

Example:

```text
PutSQL
```

Purpose:

- Execute INSERT statements
- Load data into MySQL tables

---

# Controller Services

Controller Services provide shared reusable connections.

## CassandraSessionProvider

Used to connect NiFi to Cassandra.

Configuration included:

```text
Cassandra Contact Points
Keyspace
Authentication Settings
```

---

## DBCPConnectionPool

Used to connect NiFi to MySQL.

Configuration included:

```text
JDBC URL
Driver
Username
Password
```

Benefits:

- Centralized database configuration
- Reusable across multiple processors
- Simplified connection management

---

# Docker-Based Architecture

Module 17 deployed services in Docker containers.

Example architecture:

```text
+----------------+
| Apache NiFi    |
+----------------+
        |
        |
+----------------+
| Cassandra      |
+----------------+
        |
        |
+----------------+
| MySQL          |
+----------------+
```

Benefits:

- Consistent environments
- Platform independence
- Easy deployment
- Simplified dependency management

---

# ETL Pipelines Created During Module 17

Examples completed during the module:

```text
Excel → MySQL
MongoDB → MySQL
Redis → MySQL
Cassandra → MySQL
```

Each example followed the same ETL pattern:

```text
Extract
Transform
Load
```

---

# Cassandra to MySQL ETL Pipeline

The final pipeline built during Activity 17.5:

```text
QueryCassandra
      ↓ success
SplitJSON
      ↓ split
ConvertJSONToSQL
      ↓ sql
PutSQL
```

Workflow:

```text
Cassandra
      ↓
JSON
      ↓
SQL Statements
      ↓
MySQL
```

Result:

```text
001 | Peter | Parker
```

was successfully transferred from Cassandra into MySQL.

---

# Lessons Learned

Apache NiFi can:

- Integrate with relational and NoSQL databases
- Automate ETL workflows
- Perform data transformations
- Route data between systems
- Monitor data movement in real time
- Support scalable data engineering architectures

---

# Key Takeaways

- NiFi simplifies ETL development through a graphical interface.
- Controller Services centralize external connections.
- Processors handle extraction, transformation, and loading.
- Docker provides a portable deployment platform.
- ETL pipelines can be rapidly developed and monitored using NiFi.
- Hands-on experimentation is the best way to learn NiFi.

---

# Conclusion

Module 17 demonstrated how Apache NiFi can be used to create end-to-end ETL solutions. Data was extracted from multiple sources, transformed into a target format, and loaded into relational databases. The final Cassandra-to-MySQL pipeline illustrated how NiFi integrates different technologies using a visual workflow architecture and reusable controller services.