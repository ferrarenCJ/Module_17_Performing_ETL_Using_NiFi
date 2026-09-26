# Activity 17.5: Using NiFi to Create a Pipeline in Cassandra

## Objective

The objective of this activity was to create an ETL pipeline using Apache NiFi that extracts data from a Cassandra database and loads it into a MySQL database.

---

# Environment Setup

## Docker Network

Created a shared Docker network:

```bash
docker network create NifiNetwork
```

---

## Cassandra Container

Created a Cassandra container:

```bash
docker run -p 9042:9042 --name some-cassandra --network NifiNetwork -d cassandra
```

---

## NiFi Container

Created a NiFi container:

```bash
docker run --name nificontainer -p 8080:8080 --network NifiNetwork -d apache/nifi:1.13.2
```

---

## MySQL Container

Used the existing MySQL container already available in the Docker environment:

```text
mysqlcontainer
```

---

# Step 1: Verify Containers

Verified the following containers were running:

```text
nificontainer
some-cassandra
mysqlcontainer
```

### Screenshot

```text
Step01_Cassandra_And_NiFi_Containers.png
```

---

# Step 2: Connect to Cassandra

Opened a terminal session to Cassandra:

```bash
docker exec -it some-cassandra cqlsh
```

Successful connection:

```text
Connected to Test Cluster
cqlsh>
```

### Screenshot

```text
Step02_Cassandra_CQLSH.png
```

---

# Step 3: Create Cassandra Table and Insert Data

Created the Cassandra keyspace:

```sql
CREATE KEYSPACE IF NOT EXISTS k1
WITH replication =
{
    'class':'SimpleStrategy',
    'replication_factor':'1'
};
```

Selected the keyspace:

```sql
USE k1;
```

Created the table:

```sql
CREATE TABLE person
(
    id text,
    name text,
    surname text,
    PRIMARY KEY (id)
);
```

Inserted the record:

```sql
INSERT INTO person
(
    id,
    name,
    surname
)
VALUES
(
    '001',
    'Peter',
    'Parker'
);
```

Verified the record:

```sql
SELECT *
FROM person;
```

Result:

```text
001 | Peter | Parker
```

### Screenshot

```text
Step03_Peter_Parker_Inserted.png
```

---

# Step 4: Open NiFi

Opened:

```text
http://localhost:8080/nifi
```

Verified the NiFi UI loaded successfully.

### Screenshot

```text
Step04_NiFi_UI.png
```

---

# Step 5: Create Process Group

Created a process group named:

```text
Cassandra-test
```

### Screenshot

```text
Step05_Cassandra_Test_Process_Group.png
```

---

# Step 6: Configure CassandraSessionProvider

Added a Controller Service:

```text
CassandraSessionProvider
```

Configured:

```text
Cassandra Contact Points
some-cassandra:9042
```

```text
Client Auth
NONE
```

```text
Keyspace
k1
```

### Screenshot

```text
Step06_CassandraSessionProvider_Configured.png
```

---

# Step 7: Enable CassandraSessionProvider

Enabled the service:

```text
CassandraSessionProvider
```

Status:

```text
Enabled
```

### Screenshot

```text
Step07_CassandraSessionProvider_Enabled.png
```

---

# Step 8: Configure QueryCassandra

Added:

```text
QueryCassandra
```

Configured:

```text
Cassandra Connection Provider
CassandraSessionProvider
```

```text
Client Auth
NONE
```

```text
Keyspace
k1
```

```text
Output Format
JSON
```

```sql
SELECT * FROM person
```

### Screenshot

```text
Step08_QueryCassandra_Configured.png
```

---

# Step 9: Create MySQL Database and Table

Connected to MySQL and executed:

```sql
DROP DATABASE IF EXISTS people;

CREATE DATABASE IF NOT EXISTS people;

USE people;

CREATE TABLE person
(
    id varchar(50),
    name varchar(50),
    surname varchar(50),
    PRIMARY KEY (id)
);

SELECT *
FROM person;
```

Verified the table was empty.

### Screenshot

```text
Step09_MySQL_Table_Empty.png
```

---

# Step 10: Configure DBCPConnectionPool

Added:

```text
DBCPConnectionPool
```

Configured:

```text
Database Connection URL
jdbc:mysql://mysqlcontainer:3306/people
```

```text
Database Driver Class Name
com.mysql.cj.jdbc.Driver
```

```text
Database Driver Location(s)
/opt/nifi/mysql-connector-j-8.0.33.jar
```

```text
Database User
root
```

```text
Password
password
```

Enabled the controller service successfully.

### Screenshot

```text
Step10_DBCPConnectionPool_Enabled.png
```

---

# Step 11A: Configure SplitJSON Settings

Added:

```text
SplitJSON
```

Automatically terminated:

```text
failure
original
```

### Screenshot

```text
Step11A_SplitJSON_Settings.png
```

---

# Step 11B: Configure SplitJSON Properties

Configured:

```text
JsonPath Expression
$.*
```

### Screenshot

```text
Step11B_SplitJSON_Properties.png
```

---

# Step 12: Configure ConvertJSONToSQL

Added:

```text
ConvertJSONToSQL
```

Configured:

```text
JDBC Connection Pool
DBCPConnectionPool
```

```text
Statement Type
INSERT
```

```text
Table Name
person
```

```text
Catalog Name
people
```

```text
Translate Field Names
false
```

```text
SQL Parameter Attribute Prefix
sql
```

### Screenshot

```text
Step12_ConvertJSONToSQL_Configured.png
```

---

# Step 13: Connect Processors

Connected:

```text
QueryCassandra
      ↓ success
SplitJSON
      ↓ split
ConvertJSONToSQL
```

### Screenshot

```text
Step13_QueryCassandra_SplitJSON_Connected.png
```

---

# Step 14: Add PutSQL

Added:

```text
PutSQL
```

Configured:

```text
JDBC Connection Pool
DBCPConnectionPool
```

Connected:

```text
ConvertJSONToSQL
      ↓ sql
PutSQL
```

Final pipeline:

```text
QueryCassandra
      ↓ success
SplitJSON
      ↓ split
ConvertJSONToSQL
      ↓ sql
PutSQL
```

### Screenshot

```text
Step14_All_Processors_Connected.png
```

---

# Troubleshooting

While testing the ETL process, a SQL parameter error occurred:

```text
java.sql.SQLException:
No value specified for parameter 1
```

The issue was resolved by changing:

```text
SQL Parameter Attribute Prefix
```

from:

```text
SQL
```

to:

```text
sql
```

in the ConvertJSONToSQL processor.

After updating the configuration, clearing the queues, and rerunning the flow, the pipeline executed successfully.

---

# Step 15: Verify Data Transfer

Executed:

```sql
USE people;

SELECT *
FROM person;
```

Result:

```text
+-----+--------+---------+
| id  | name   | surname |
+-----+--------+---------+
| 001 | Peter  | Parker  |
+-----+--------+---------+
```

The record was successfully transferred from Cassandra to MySQL using Niagara Files (NiFi).

### Screenshot

```text
Step15_MySQL_Data_Loaded.png
```

---

# Conclusion

This activity successfully demonstrated how to use Apache NiFi to build a Cassandra-to-MySQL ETL pipeline. Data was extracted from a Cassandra database, transformed into SQL statements using ConvertJSONToSQL, and loaded into a MySQL database using PutSQL. The final verification confirmed that the record for Peter Parker was successfully moved from Cassandra to MySQL.