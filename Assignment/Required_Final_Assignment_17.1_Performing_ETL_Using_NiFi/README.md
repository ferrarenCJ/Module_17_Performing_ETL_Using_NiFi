# Required Final Assignment 17.1: Performing ETL Using NiFi

## Student Information

**Module:** Module 17 - Performing ETL Using NiFi  
**Assignment:** Required Final Assignment 17.1 - Performing ETL Using NiFi

---

# Objective

The objective of this assignment was to create and execute ETL pipelines using Apache NiFi. Two separate ETL workflows were developed:

1. Convert data from an Excel workbook into a CSV file.
2. Load data from a CSV file into a MySQL database.

The assignment demonstrated the complete ETL process of data extraction, transformation, and loading using Apache NiFi processors, controller services, and Docker-based infrastructure.

---

# Environment Setup

## Docker Containers

The following Docker containers were used:

```text
nificontainer
mysqlcontainer
```

## Docker Network

The containers were connected through Docker networking to enable communication between NiFi and MySQL.

---

# Part 1: Writing Data to a CSV File

## Step 1: Create Input and Output Directories

Opened the NiFi container terminal and created the required directories.

Commands executed:

```bash
docker exec -it nificontainer bash

cd /opt/nifi/nifi-current

mkdir input
mkdir output
```

### Screenshot

**Step01_Input_Output_Folders.png**

**Caption:**

> Step 1 - Created the input and output directories inside the NiFi container under `/opt/nifi/nifi-current`.

---

## Step 2: Copy Excel File into NiFi

Copied the provided Excel file into the input directory.

Command executed:

```bash
docker cp ./movies.xlsx nificontainer:/opt/nifi/nifi-current/input
```

Verified file placement:

```bash
cd /opt/nifi/nifi-current/input

ls
```

### Screenshot

**Step02_MoviesXLSX_Copied.png**

**Caption:**

> Step 2 - Successfully copied the `movies.xlsx` file into the NiFi input directory and verified its presence.

---

## Step 3: Create Assignment17 Process Group

Opened the NiFi UI and created a process group named:

```text
Assignment17
```

### Screenshot

**Step03_Assignment17_ProcessGroup.png**

**Caption:**

> Step 3 - Created the Assignment17 process group within the NiFi canvas to contain the ETL pipelines developed for Assignment 17.1.

---

## Step 4: Configure GetFile Processor

Added a GetFile processor.

### Scheduling

```text
Run Schedule = 15 sec
```

### Properties

```text
Input Directory
/opt/nifi/nifi-current/input
```

```text
File Filter
movies.xlsx
```

### Screenshot

**Step04_GetFile_Configured.png**

**Caption:**

> Step 4 - Configured the GetFile processor with a 15-second run schedule, input directory `/opt/nifi/nifi-current/input`, and file filter `movies.xlsx`.

---

## Step 5: Configure ConvertExcelToCSVProcessor

Added the ConvertExcelToCSVProcessor.

### Scheduling

```text
Run Schedule = 15 sec
```

### Properties

```text
Sheets to Extract
Sheet 1 - movies
```

### Screenshot

**Step05_ConvertExcelToCSV_Configured.png**

**Caption:**

> Step 5 - Configured the ConvertExcelToCSVProcessor with a 15-second run schedule and configured the processor to extract data from the worksheet "Sheet 1 - movies".

---

## Step 6: Configure PutFile Processor

Added a PutFile processor.

### Settings

```text
Automatically Terminate Relationships

success
```

### Scheduling

```text
Run Schedule = 15 sec
```

### Properties

```text
Directory
/opt/nifi/nifi-current/output
```

### Screenshot

**Step06_PutFile_Configured.png**

**Caption:**

> Step 6 - Configured the PutFile processor with a 15-second run schedule, success relationship auto-termination, and output directory `/opt/nifi/nifi-current/output`.

---

## Step 7: Connect Processors

Configured the following workflow:

```text
GetFile
    ↓ success
ConvertExcelToCSVProcessor
    ↓ success
PutFile
```

### Screenshot

**Step07_Pipeline_Connected.png**

**Caption:**

> Step 7 - Connected the GetFile, ConvertExcelToCSVProcessor, and PutFile processors using the required relationships to create the Excel-to-CSV ETL pipeline.

---

## Step 8: Execute Pipeline

Started all processors:

```text
GetFile
ConvertExcelToCSVProcessor
PutFile
```

Verified successful execution.

### Screenshot

**Step08_Pipeline_Running.png**

**Caption:**

> Step 8 - Started the GetFile, ConvertExcelToCSVProcessor, and PutFile processors and verified that the Excel-to-CSV ETL pipeline executed successfully.

---

## Step 9: Verify CSV Output

Verified CSV file creation in the output directory.

Command executed:

```bash
cd /opt/nifi/nifi-current/output

ls -l *.csv
```

Generated file:

```text
movies_Sheet 1 - movies.csv
```

### Screenshot

**Step09_Movies_Assignment_CSV_Created.png**

**Caption:**

> Step 9 - Verified that the Excel-to-CSV ETL pipeline successfully generated the file `movies_Sheet 1 - movies.csv` in the `/opt/nifi/nifi-current/output` directory.

---

# Part 2: Writing Data to a MySQL Database

## Step 10: Create MySQL Database and Table

Created the target database and table.

```sql
DROP DATABASE IF EXISTS movielens;

CREATE DATABASE movielens;

USE movielens;

CREATE TABLE movies (
    idx INT,
    title VARCHAR(100),
    genres VARCHAR(100)
);

SELECT *
FROM movies;
```

Verified that the table was empty.

### Screenshot

**Step10_Empty_Movies_Table.png**

**Caption:**

> Step 10 - Created the `movielens` database and initialized the `movies` table with columns `idx`, `title`, and `genres`, confirming that the table was initially empty.

---

## Step 11: Copy CSV File into NiFi

Created the data directory and copied the CSV file.

```bash
docker cp ./movies.csv nificontainer:/opt/nifi/nifi-current/data
```

Verified:

```bash
ls -lh
```

### Screenshot

**Step11_MoviesCSV_On_NiFi_Server.png**

**Caption:**

> Step 11 - Copied the `movies.csv` file into the `/opt/nifi/nifi-current/data` directory inside the NiFi container and verified that the file was successfully uploaded for processing.

---

## Step 12: Open NiFi UI

Opened the Apache NiFi interface and accessed Assignment17.

### Screenshot

**Step12_NiFi_UI.png**

**Caption:**

> Step 12 - Opened the Apache NiFi user interface and accessed the Assignment17 process group to begin configuring the CSV-to-MySQL ETL pipeline.

---

## Step 13: Configure MySQL Controller Service

Created a DBCPConnectionPool controller service named:

```text
MySQL
```

### Configuration

```text
Database Connection URL
jdbc:mysql://mysqlcontainer:3306/movielens
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

**Step13_MySQL_Controller_Enabled.png**

**Caption:**

> Step 13 - Created and enabled the MySQL DBCPConnectionPool controller service to establish connectivity between Apache NiFi and the movielens database.

---

## Step 14: Configure Reader and Writer Services

Created:

```text
CSVRead
```

and:

```text
JsonRecordWriter
```

Enabled:

```text
CSVRead
MySQL
JsonRecordWriter
```

### Screenshot

**Step14_Controller_Services_Enabled.png**

**Caption:**

> Step 14 - Created and enabled the CSVRead, JsonRecordWriter, and MySQL controller services required to support the CSV-to-MySQL ETL pipeline.

---

## Step 15: Create ETL Pipeline

Added the following processors:

```text
GetFile
SplitText
ConvertRecord
ConvertJSONToSQL
PutSQL
```

### Screenshot

**Step15_Complete_Pipeline.png**

**Caption:**

> Part 2 Step 6 - Added the five required processors (GetFile, SplitText, ConvertRecord, ConvertJSONToSQL, and PutSQL) that will be used to load movie data from a CSV file into the movielens MySQL database.

---

## Step 16: Configure and Connect Pipeline

### Processor Flow

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

### Configuration Summary

#### GetFile

```text
Input Directory
/opt/nifi/nifi-current/data
```

#### SplitText

```text
Line Split Count = 1
Header Line Count = 1
```

#### ConvertRecord

```text
Record Reader = CSVRead
Record Writer = JsonRecordWriter
```

#### ConvertJSONToSQL

```text
JDBC Connection Pool = MySQL
Statement Type = INSERT
Table Name = movies
Catalog Name = movielens
SQL Parameter Attribute Prefix = sql
```

#### PutSQL

```text
JDBC Connection Pool = MySQL
```

### Screenshot

**Step16_Pipeline_Connected.png**

**Caption**

> Part 2 Step 7 - Configured and connected all processors using the required relationships to create the complete CSV-to-MySQL ETL workflow.

---

## Step 17: Execute CSV-to-MySQL Pipeline

Started:

```text
GetFile
SplitText
ConvertRecord
ConvertJSONToSQL
PutSQL
```

Verified pipeline execution.

### Screenshot

**Step17_All_Processors_Running.png**

**Caption**

> Part 2 Step 8 - Started the GetFile, SplitText, ConvertRecord, ConvertJSONToSQL, and PutSQL processors and verified that the CSV-to-MySQL ETL pipeline was running successfully.

---

## Step 18: Verify MySQL Database Load

Executed:

```sql
USE movielens;

SELECT *
FROM movies;
```

Results returned:

```text
1000 movie records
```

Example records:

```text
Moby Dick (1956)
Creature from the Black Lagoon, The (1954)
Ice Castles (1978)
Entrapment (1999)
```

### Screenshot

**Step18_Movies_Table_Loaded.png**

**Caption**

> Part 2 Step 9 - Verified that movie records from movies.csv were successfully loaded into the movielens.movies table using the Apache NiFi ETL pipeline. The query returned 1,000 movie records.

---

# Conclusion

This assignment successfully demonstrated how Apache NiFi can be used to perform ETL operations across multiple stages of a data pipeline. In Part 1, data was extracted from an Excel workbook, transformed, and written to a CSV file. In Part 2, the CSV data was processed, transformed into SQL statements, and loaded into a MySQL database. The successful insertion of 1,000 movie records into the `movielens.movies` table verified that both ETL pipelines operated correctly and that Apache NiFi can effectively orchestrate data movement between different storage systems.