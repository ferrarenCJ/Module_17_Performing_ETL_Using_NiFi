# Required Coding Activity 17.1: Using NiFi to Connect to an Excel File

## Learning Outcome Addressed

4. Use NiFi to create an ETL pipeline.

---

# Objective

Build a complete ETL workflow using Apache NiFi that:

1. Reads an Excel file
2. Converts the Excel file to CSV format
3. Stores the resulting CSV file in an output directory

---

# ETL Workflow

```text
Activity17_1.xlsx
        │
        ▼
     GetFile
        │
        ▼
ConvertExcelToCSVProcessor
        │
        ▼
     PutFile
        │
        ▼
       CSV
```

---

# Environment Prerequisites

## Docker Network

```bash
docker network create NifiNetwork
```

Verify:

```bash
docker network ls
```

---

## NiFi Container

```bash
docker run --name nificontainer \
-p 8080:8080 \
--network NifiNetwork \
-d apache/nifi:1.13.2
```

Verify:

```bash
docker ps
```

---

# Step 1 - Create Input and Output Directories

Open the NiFi container terminal.

Navigate to:

```bash
cd /opt/nifi/nifi-current
```

Create folders:

```bash
mkdir input
mkdir output
```

Verify:

```bash
ls
```

Expected:

```text
input
output
```

---

## Screenshot Required

### Screenshot 1

Show:

```text
input
output
```

folders successfully created.

---

# Step 2 - Copy Excel File into Container

Download:

```text
Activity17_1.xlsx
```

Copy to container:

```bash
docker cp Activity17_1.xlsx \
nificontainer:/opt/nifi/nifi-current/input
```

Verify:

```bash
ls input
```

Expected:

```text
Activity17_1.xlsx
```

---

## Screenshot Required

### Screenshot 2

Show:

```text
Activity17_1.xlsx
```

inside the input folder.

---

# Step 3 - Open NiFi

Navigate to:

```text
http://localhost:8080/nifi
```

Expected:

```text
NiFi Canvas
```

---

## Screenshot Required

### Screenshot 3

Show:

```text
NiFi successfully loaded in browser
```

---

# Step 4 - Create Process Group

Drag:

```text
Process Group
```

onto the canvas.

Name:

```text
Activity17.1
```

Select:

```text
Add
```

---

## Screenshot Required

### Screenshot 4

Show:

```text
Activity17.1
```

process group created on canvas.

---

# Step 5 - Add ConvertExcelToCSVProcessor

Open:

```text
Activity17.1
```

Double-click the process group.

Drag a Processor onto the canvas.

Select:

```text
ConvertExcelToCSVProcessor
```

---

## Screenshot Required

### Screenshot 5

Show:

```text
ConvertExcelToCSVProcessor
```

on canvas.

---

# Step 6 - Configure ConvertExcelToCSVProcessor

Right-click:

```text
ConvertExcelToCSVProcessor
```

Select:

```text
Configure
```

---

## Scheduling Tab

Set:

```text
Run Schedule = 60 sec
```

---

## Properties Tab

Set:

```text
Sheets to Extract = Test
```

---

## Screenshot Required

### Screenshot 6

Show:

```text
Sheets to Extract = Test
```

configured correctly.

---

# Step 7 - Add GetFile Processor

Add processor:

```text
GetFile
```

---

## Screenshot Required

### Screenshot 7

Show:

```text
GetFile
```

on canvas.

---

# Step 8 - Configure GetFile Processor

Open:

```text
Configure
```

---

## Scheduling Tab

Set:

```text
Run Schedule = 60 sec
```

---

## Properties Tab

Input Directory:

```text
/opt/nifi/nifi-current/input
```

File Filter:

```text
Activity17_1.xlsx
```

---

## Screenshots Required

### Screenshot 8A

Show:

```text
Properties Tab
```

with:

```text
Input Directory
File Filter
```

configured.

---

### Screenshot 8B

Show:

```text
Scheduling Tab
```

with:

```text
Run Schedule = 60 sec
```

---

# Step 9 - Add PutFile Processor

Add:

```text
PutFile
```

processor.

---

## Screenshot Required

### Screenshot 9

Show:

```text
PutFile
```

on canvas.

---

# Step 10 - Configure PutFile Processor

Open:

```text
Configure
```

---

## Properties Tab

Directory:

```text
/opt/nifi/nifi-current/output
```

---

## Settings Tab

Automatically Terminate Relationships:

```text
success
```

selected.

---

## Scheduling Tab

Set:

```text
Run Schedule = 60 sec
```

---

## Screenshots Required

### Screenshot 10A

Show:

```text
Directory = /opt/nifi/nifi-current/output
```

---

### Screenshot 10B

Show:

```text
success
```

relationship terminated.

---

# Step 11 - Connect GetFile to ConvertExcelToCSVProcessor

Create connector:

```text
GetFile
      │
      ▼
ConvertExcelToCSVProcessor
```

---

## Screenshot Required

### Screenshot 11

Show successful connection.

---

# Step 12 - Connect ConvertExcelToCSVProcessor to PutFile

Create connector:

```text
ConvertExcelToCSVProcessor
                │
                ▼
             PutFile
```

Relationships:

```text
failure
original
success
```

---

## Screenshot Required

### Screenshot 12

Show connection and relationship settings.

---

# Final Workflow

```text
GetFile
    │
    ▼
ConvertExcelToCSVProcessor
    │
    ▼
PutFile
```

---

# Step 13 - Start GetFile Processor

Start:

```text
GetFile
```

Expected:

```text
Green Arrow
```

File should disappear from:

```text
input
```

folder.

---

## Screenshots Required

### Screenshot 13A

Show:

```text
GetFile running
```

---

### Screenshot 13B

Show:

```text
input folder empty
```

---

# Step 14 - Start ConvertExcelToCSVProcessor

Start:

```text
ConvertExcelToCSVProcessor
```

Expected:

```text
Green Arrow
```

---

## Screenshot Required

### Screenshot 14

Show processor running.

---

# Step 15 - Start PutFile Processor

Start:

```text
PutFile
```

Expected:

```text
Green Arrow
```

---

## Screenshot Required

### Screenshot 15

Show processor running.

---

# Step 16 - Verify CSV Output

Open NiFi container terminal.

Navigate:

```bash
cd /opt/nifi/nifi-current/output
```

List files:

```bash
ls
```

Expected:

```text
Activity17_1.xlsx
Activity17_1_Test.csv
```

or similar output depending on sheet naming.

---

## Screenshot Required

### Screenshot 16

Show generated CSV file.

---

# ETL Mapping

| ETL Stage | Processor |
|------------|------------|
| Extract | GetFile |
| Transform | ConvertExcelToCSVProcessor |
| Load | PutFile |

---

# Submission Checklist

## Required Screenshots

- [ ] Screenshot 1 - input/output folders created
- [ ] Screenshot 2 - Excel file copied to container
- [ ] Screenshot 3 - NiFi browser page
- [ ] Screenshot 4 - Activity17.1 process group
- [ ] Screenshot 5 - ConvertExcelToCSVProcessor added
- [ ] Screenshot 6 - ConvertExcelToCSVProcessor configured
- [ ] Screenshot 7 - GetFile processor added
- [ ] Screenshot 8A - GetFile properties
- [ ] Screenshot 8B - GetFile scheduling
- [ ] Screenshot 9 - PutFile processor added
- [ ] Screenshot 10A - PutFile properties
- [ ] Screenshot 10B - PutFile settings
- [ ] Screenshot 11 - GetFile connected to ConvertExcelToCSVProcessor
- [ ] Screenshot 12 - ConvertExcelToCSVProcessor connected to PutFile
- [ ] Screenshot