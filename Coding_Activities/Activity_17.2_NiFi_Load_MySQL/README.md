# Required Coding Activity 17.2: Using NiFi to Load Data to MySQL

## Overview

This activity demonstrates how to use Apache NiFi to create an ETL pipeline that loads earthquake data from a CSV file into a MySQL database. The dataset was provided by the United States Geological Survey (USGS) and was loaded into a MySQL table using NiFi processors and controller services.

---

## Learning Outcome

- Use NiFi to create an ETL pipeline.
- Configure MySQL and NiFi integration.
- Load CSV data into a relational database.
- Validate successful data ingestion.

---

## Environment

### Docker Containers

- Apache NiFi 1.13.2
- MySQL 8.4.11

### Database

```sql
Database Name: usgs
Table Name: earthquakes
```

### JDBC Driver

```text
mysql-connector-j-8.0.33.jar
```

---

## Step 1: Create the MySQL Database and Table

Created the `usgs` database and the `earthquakes` table using MySQL Workbench.

### SQL

```sql
CREATE DATABASE IF NOT EXISTS usgs;

USE usgs;

CREATE TABLE earthquakes(
    idx int,
    time varchar(100),
    latitude varchar(100),
    longitude varchar(100),
    depth varchar(100),
    mag varchar(100),
    magType varchar(100),
    nst varchar(100),
    gap varchar(100),
    dmin varchar(100),
    rms varchar(100),
    net varchar(100),
    id varchar(100),
    updated varchar(100),
    place varchar(100),
    type varchar(100),
    horizontalError varchar(100),
    depthError varchar(100),
    magError varchar(100),
    magNst varchar(100),
    status varchar(100),
    locationSource varchar(100),
    magSource varchar(100)
);
```

### Screenshot

**Step01_Empty_Earthquakes_Table.png**

Successfully created and verified the empty earthquakes table.

---

## Step 2: Copy the CSV File to the NiFi Server

Created the required directory structure inside the NiFi container:

```bash
/opt/nifi/current-nifi/data
```

Copied the file:

```text
Activity17-2.csv
```

to the NiFi server using:

```bash
docker cp Activity17-2.csv nificontainer:/opt/nifi/current-nifi/data
```

Verified the file existed within the container.

### Screenshot

**Step02_CSV_File_On_NiFi_Server.png**

Verified the CSV file was successfully copied to the NiFi server.

---

## Step 3: Configure Controller Services

Created and enabled the following Controller Services:

### MySQL

```text
DBCPConnectionPool
```

Configuration:

```text
Database Connection URL:
jdbc:mysql://mysqlcontainer:3306/usgs

Database Driver Class Name:
com.mysql.cj.jdbc.Driver

Database Driver Location:
 /opt/nifi/mysql-connector-j-8.0.33.jar

Database User:
root

Password:
password
```

### Record Reader

```text
CSVReader
```

### Record Writer

```text
JsonRecordSetWriter
```

All services were successfully enabled.

### Screenshot

**Step03_Controller_Services_Enabled.png**

---

## Step 4: Create the NiFi Data Pipeline

Added the following processors:

1. GetFile
2. SplitText
3. ConvertRecord
4. ConvertJSONToSQL
5. PutSQL

### Pipeline Layout

```text
GetFile
   ↓
SplitText
   ↓
ConvertRecord
   ↓
ConvertJSONToSQL
   ↓
PutSQL
```

### Screenshot

**Step04_Five_Processor_Pipeline.png**

---

## Step 5: Configure Processor Connections

Configured the required relationships:

### GetFile → SplitText

```text
success
```

### SplitText → ConvertRecord

```text
splits
```

### ConvertRecord → ConvertJSONToSQL

```text
success
```

### ConvertJSONToSQL → PutSQL

```text
sql
```

### PutSQL → PutSQL

```text
retry
```

### Connection Diagram

```text
GetFile
   ↓ success
SplitText
   ↓ splits
ConvertRecord
   ↓ success
ConvertJSONToSQL
   ↓ sql
PutSQL
   ↺ retry
```

### Screenshot

**Step05_Pipeline_Connections.png**

---

## Step 6: Execute the Pipeline

Started all processors and monitored the data flow through the pipeline.

To improve throughput, the following settings were updated for:

- ConvertRecord
- ConvertJSONToSQL
- PutSQL

### Scheduling

```text
Concurrent Tasks = 5
Run Schedule = 0 sec
```

The pipeline successfully processed all earthquake records.

### Screenshot

**Step06_All_Processors_Running.png**

---

## Step 7: Verify Data Loaded into MySQL

Validated that earthquake records were successfully loaded into the database.

### Verification Query

```sql
SELECT COUNT(*) AS row_count
FROM earthquakes;
```

### Result

```text
58 records loaded
```

This confirms that the NiFi ETL pipeline successfully imported all earthquake records from the CSV file into the MySQL database.

### Screenshot

**Step07_Earthquakes_Table_Loaded.png**

---

## Conclusion

This activity successfully demonstrated how Apache NiFi can be used to perform ETL processing and load CSV data into a MySQL database. The solution included configuring Controller Services, creating a five-stage data pipeline, connecting processors through relationships, and loading 58 earthquake records into the `usgs.earthquakes` table.

### Final Results

✅ MySQL database created

✅ Earthquakes table created

✅ CSV file copied to NiFi

✅ Controller Services configured

✅ ETL pipeline created

✅ Data successfully loaded into MySQL

✅ 58 earthquake records verified