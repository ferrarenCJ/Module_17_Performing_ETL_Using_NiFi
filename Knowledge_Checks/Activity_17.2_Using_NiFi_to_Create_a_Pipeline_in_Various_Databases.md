# Self-Study Drag & Drop Activity 17.2: Using NiFi to Create a Pipeline in Various Databases

## Learning Outcome Addressed

### 4. Use NiFi to create an ETL pipeline.

**Estimated Time:** 25 minutes  
**Type:** Optional Self-Study Activity  
**Grading:** Does not count toward the final course grade.

---

# Objective

This activity reinforced the fundamental concepts of building ETL pipelines using Apache NiFi across various database technologies and data sources.

The exercise focused on identifying the key components required to build an ETL workflow and understanding the sequence of steps necessary to move data from a source system to a target system using NiFi.

---

# Completed Activity

## Completed Paragraph

For most of the database types you have learned about so far, an ETL pipeline using **NiFi** can be constructed using **containers**. This is true for data that is stored in a(n) **Excel** file, or in a MySQL or **Cassandra** type of database. However, when working with a **Redis** database, you must use a **standalone** installation of Redis and NiFi in order to construct an **ETL** pipeline.

To perform ETL using NiFi on a MySQL or Cassandra database, the first thing you need to do is create a Docker **network** and then connect a NiFi container and a MySQL or Cassandra container to it, depending on the type of **database** you are working on.

The next step is to open the NiFi **UI** and create the necessary components to perform ETL. This can include creating **controllers** and processors that will define the operations on your data from one database to another.

Next, you will need to connect the processors created above in order to define the **flow** of your data from one database to another.

Finally, after you have all of your components in place and connected them successfully, you will just need to **start** the flow and ensure that the data is transferred correctly between **databases**.

---

# Answer Key

| Blank | Answer |
|---------|---------|
| 1 | NiFi |
| 2 | containers |
| 3 | Excel |
| 4 | Cassandra |
| 5 | Redis |
| 6 | standalone |
| 7 | ETL |
| 8 | network |
| 9 | database |
| 10 | UI |
| 11 | controllers |
| 12 | flow |
| 13 | start |
| 14 | databases |

---

# Key Concepts Reinforced

## ETL Pipeline

ETL stands for:

```text
Extract
Transform
Load
```

A typical ETL workflow follows:

```text
Source Data
     ↓
Extract
     ↓
Transform
     ↓
Load
     ↓
Target Database
```

---

## Apache NiFi

Apache NiFi provides a graphical interface that enables users to:

- Create ETL pipelines
- Connect multiple data sources
- Transform data
- Route data between systems
- Monitor data movement in real time

---

## Controller Services

Controller Services provide reusable configurations and connections.

Examples include:

```text
DBCPConnectionPool
CassandraSessionProvider
```

Benefits:

- Centralized configuration
- Reusable database connections
- Simplified maintenance

---

## Docker Networking

When using NiFi with databases running in containers:

```text
Docker Network
      ↓
NiFi Container
      ↓
Database Container
```

Containers must be attached to the same Docker network to communicate with each other.

---

## Data Sources Used in Module 17

Examples of ETL pipelines created during this module:

```text
Excel → MySQL
MongoDB → MySQL
Redis → MySQL
Cassandra → MySQL
```

Each pipeline follows the same ETL process with different source systems.

---

# Summary

This activity reviewed the steps required to build ETL pipelines using Apache NiFi. Key concepts included Docker networking, Controller Services, processor connections, and defining the flow of data between source and target systems. The activity reinforced how NiFi simplifies ETL development through a visual workflow environment and reusable components.