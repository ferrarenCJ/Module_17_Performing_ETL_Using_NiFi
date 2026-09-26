# Module 17: Wrap-Up

## Overview

In this module, I learned how to use **Apache NiFi** as a data pipeline and transformation platform to perform **ETL (Extract, Transform, Load)** operations across multiple data sources and database technologies.

The module expanded on ETL concepts introduced in previous modules and introduced the architecture, features, and capabilities of NiFi. Through a series of hands-on activities, I learned how to design, configure, and execute ETL pipelines that move data between source systems and target databases.

---

# What I Learned

## Understanding Apache NiFi

Apache NiFi is a visual data integration and workflow orchestration tool that simplifies building and managing ETL pipelines.

Key capabilities include:

- Visual drag-and-drop workflow design
- Data ingestion and routing
- Data transformation
- Real-time monitoring
- Integration with multiple data sources
- Support for relational and NoSQL databases

---

## NiFi Architecture

The module introduced the major components of NiFi:

### FlowFiles

FlowFiles are the data objects that move through a NiFi workflow.

They contain:

- Content
- Attributes (metadata)

FlowFiles are passed between processors as data moves through the pipeline.

---

### Processors

Processors perform operations on FlowFiles.

Examples used during the module included:

```text
QueryCassandra
QueryMongo
SplitJSON
ConvertJSONToSQL
PutSQL
```

Processors enable data extraction, transformation, and loading.

---

### Connections

Connections define how data flows between processors.

Functions include:

- Routing data
- Managing queues
- Buffering FlowFiles
- Handling success and failure relationships

---

### Controller Services

Controller Services provide reusable configurations for connecting to external systems.

Examples:

```text
DBCPConnectionPool
```

Used for:

```text
MySQL connectivity
```

and:

```text
CassandraSessionProvider
```

Used for:

```text
Cassandra connectivity
```

---

# Building ETL Pipelines

The module demonstrated how to design complete ETL solutions using NiFi.

General workflow:

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

Typical NiFi implementation:

```text
Query Processor
      ↓
Transformation Processor
      ↓
Load Processor
```

---

# Environment Setup

Multiple ETL environments were created using Docker.

Activities included:

### Creating Containers

Examples:

```text
NiFi Container
MySQL Container
MongoDB Container
Redis Container
Cassandra Container
```

### Creating Docker Networks

Containers were connected through shared Docker networks to enable communication between services.

### Installing Drivers

JDBC drivers were installed and configured to allow database connectivity between NiFi and relational databases.

---

# Database Technologies Used

The module demonstrated ETL processes using several data sources.

Examples included:

```text
Excel
MySQL
MongoDB
Redis
Cassandra
```

This provided experience working with both relational and NoSQL databases.

---

# Cassandra-to-MySQL ETL Pipeline

The final activity used Apache NiFi to move data from Cassandra into MySQL.

Pipeline:

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

was successfully transferred from Cassandra into the MySQL database.

---

# Key Skills Developed

Throughout Module 17, I gained experience with:

- Building ETL pipelines
- Configuring Controller Services
- Creating FlowFiles
- Connecting processors
- Managing processor relationships
- Creating Docker-based environments
- Using JDBC connections
- Monitoring ETL workflows
- Debugging NiFi pipelines
- Integrating relational and NoSQL databases

---

# Important Takeaways

- Apache NiFi provides a low-code approach to ETL development.
- ETL workflows can be created visually using processors and connections.
- Docker simplifies deployment of databases and supporting services.
- Controller Services centralize reusable connection configurations.
- NiFi supports a variety of database technologies and data formats.
- Monitoring and troubleshooting are important parts of building reliable ETL pipelines.

---

# Conclusion

Module 17 provided practical experience designing and implementing ETL pipelines using Apache NiFi. Beginning with NiFi fundamentals and progressing through multiple database integrations, the module demonstrated how data can be extracted, transformed, and loaded across different systems. By the end of the module, a complete Cassandra-to-MySQL ETL pipeline was successfully implemented, reinforcing both ETL concepts and modern data integration practices.