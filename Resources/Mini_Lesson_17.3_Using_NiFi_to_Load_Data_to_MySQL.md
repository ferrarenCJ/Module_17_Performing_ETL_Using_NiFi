# Mini-Lesson 17.3: Using NiFi to Load Data to MySQL

## Overview

Data engineers routinely load large volumes of data into databases. Data often arrives as:

- CSV files
- Text files
- Excel files
- JSON files
- XML files

Before data can be analyzed, it must be loaded into a database.

Apache NiFi provides processors that automate loading data into MySQL using an ETL pipeline.

---

# Prerequisites

Before loading data into MySQL:

## MySQL Container

A MySQL container must already be installed and running.

## Database Created

Example:

```sql
CREATE DATABASE earthquakes;
```

## Target Table Created

Example:

```sql
CREATE TABLE earthquakes (
    time VARCHAR(50),
    latitude FLOAT,
    longitude FLOAT,
    depth FLOAT,
    mag FLOAT
);
```

## NiFi Running

```bash
docker run --name nificontainer \
-p 8080:8080 \
--network NifiNetwork \
-d apache/nifi:1.13.2
```

---

# NiFi Load Pipeline

The MySQL pipeline contains five processors:

```text
GetFile
   │
   ▼
SplitText
   │
   ▼
ConvertRecord
   │
   ▼
ConvertJSONToSQL
   │
   ▼
PutSQL
```

---

# Processor 1: GetFile

## Purpose

Reads a file from a server directory and creates a FlowFile.

## Input

```text
CSV File
```

## Output

```text
FlowFile
```

## Typical Configuration

```text
Input Directory:
/opt/nifi/nifi-current/input
```

---

# Processor 2: SplitText

## Purpose

Splits the CSV file into separate FlowFiles.

## Example

### CSV File

```csv
1,John
2,Mary
3,Bob
```

### Result

```text
FlowFile #1
FlowFile #2
FlowFile #3
```

## Benefit

Allows:

- Faster processing
- Parallel execution
- Higher throughput

---

# Processor 3: ConvertRecord

## Purpose

Converts CSV data into JSON records.

## Input

```csv
1,John
```

## Output

```json
{
  "id": 1,
  "name": "John"
}
```

## Required Controller Services

### CSVReader

Reads CSV content.

### JSONRecordSetWriter

Writes JSON content.

---

# Processor 4: ConvertJSONToSQL

## Purpose

Generates SQL statements from JSON data.

## Input

```json
{
  "id": 1,
  "name": "John"
}
```

## Output

```sql
INSERT INTO employees
VALUES (1,'John');
```

## Notes

This processor prepares SQL commands for database loading.

---

# Processor 5: PutSQL

## Purpose

Executes SQL statements against MySQL.

## Example

```sql
INSERT INTO employees
VALUES (1,'John');
```

The command is executed against the database.

---

# End-to-End Flow Example

## Source CSV

```csv
time,latitude,longitude
2021-01-01,34.5,-117.3
2021-01-02,35.1,-118.2
```

---

## SplitText

```text
FlowFile #1
FlowFile #2
```

---

## ConvertRecord

```json
{
  "time":"2021-01-01",
  "latitude":34.5,
  "longitude":-117.3
}
```

---

## ConvertJSONToSQL

```sql
INSERT INTO earthquakes
VALUES (...);
```

---

## PutSQL

```text
Row inserted into MySQL table
```

---

# Performance Considerations

The PutSQL processor is typically the bottleneck.

Reason:

```text
1 CSV Record
=
1 SQL INSERT
```

Large files can generate thousands or millions of INSERT statements.

---

# Common Performance Improvements

## Increase Concurrent Tasks

Example:

```text
Concurrent Tasks = 4
```

or

```text
Concurrent Tasks = 8
```

---

## Increase Batch Size

Allows more records to be committed in a single transaction.

---

## Multiple Threads

Improves loading performance for large datasets.

---

# ETL Mapping

| ETL Stage | NiFi Processor |
|------------|------------|
| Extract | GetFile |
| Transform | SplitText |
| Transform | ConvertRecord |
| Transform | ConvertJSONToSQL |
| Load | PutSQL |

---

# Key Takeaways

✅ GetFile reads source files

✅ SplitText creates one FlowFile per row

✅ ConvertRecord transforms CSV into JSON

✅ ConvertJSONToSQL generates SQL commands

✅ PutSQL inserts records into MySQL

✅ PutSQL is generally the performance bottleneck

✅ Threading and batching improve load performance

---

# Expected Activity 17.2 Workflow

```text
CSV File
   │
   ▼
GetFile
   │
   ▼
SplitText
   │
   ▼
ConvertRecord
   │
   ▼
ConvertJSONToSQL
   │
   ▼
PutSQL
   │
   ▼
MySQL Database
```

---

# Relationship to Activity 17.1

Activity 17.1 focused on:

```text
Excel
   ▼
CSV
```

using:

```text
GetFile
ConvertExcelToCSVProcessor
PutFile
```

Activity 17.2 extends the workflow by taking the CSV output and loading it into a MySQL database using:

```text
GetFile
SplitText
ConvertRecord
ConvertJSONToSQL
PutSQL
```

This completes the ETL process by loading the transformed data into a relational database.