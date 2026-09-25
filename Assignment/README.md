# Required Coding Activity 17.1: Using NiFi to Connect to an Excel File

## Objective

Create an Apache NiFi ETL pipeline that:

1. Reads an Excel file
2. Converts the Excel file to CSV format
3. Stores the CSV output in a target directory

This activity demonstrates the Extract, Transform, and Load (ETL) process using Apache NiFi and Docker.

---

## Learning Outcome

- Use NiFi to create an ETL pipeline.

---

## Environment

### Docker Network

```bash
docker network create NifiNetwork
```

### NiFi Container

```bash
docker run --name nificontainer -p 8080:8080 --network NifiNetwork -d apache/nifi:1.13.2
```

### NiFi URL

```text
http://localhost:8080/nifi
```

---

## Assignment Files

```text
Assignment_Files/
├── Activity17_1.xlsx
└── Required_Coding_Activity_17.1-Using_NiFi_to_Connect_to_an_Excel_File_starter.zip
```

---

## Folder Structure

```text
Assignment/
├── Assignment_Files/
├── Screenshots/
├── Assignment_17.1_Submission.docx
└── README.md
```

---

## NiFi Workflow

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

## Configuration

### GetFile

#### Properties

```text
Input Directory:
/opt/nifi/nifi-current/input
```

```text
File Filter:
Activity17_1.xlsx
```

#### Scheduling

```text
Run Schedule = 60 sec
```

---

### ConvertExcelToCSVProcessor

#### Properties

```text
Sheets to Extract:
Activity17_1
```

> Note: The Mini-Lesson used `Test`, but the assignment Excel file contains a worksheet named `Activity17_1`. The processor must match the actual worksheet name.

#### Scheduling

```text
Run Schedule = 60 sec
```

---

### PutFile

#### Properties

```text
Directory:
/opt/nifi/nifi-current/output
```

#### Settings

```text
Automatically Terminate Relationships

✓ success
✓ failure
```

#### Scheduling

```text
Run Schedule = 60 sec
```

---

## Connections

### Connection 1

```text
GetFile
    ↓
ConvertExcelToCSVProcessor
```

Relationship:

```text
success
```

---

### Connection 2

```text
ConvertExcelToCSVProcessor
                  ↓
               PutFile
```

Relationships:

```text
failure
original
success
```

---

## Screenshots Collected

### Step 1

```text
Step01_Input_Output_Folders.png
```

Created:

```text
input
output
```

folders.

---

### Step 2

```text
Step02_Activity17_1_File_Copied.png
```

Copied:

```text
Activity17_1.xlsx
```

into the NiFi input directory.

---

### Step 3

```text
Step03_NiFi_Home_Page.png
```

Opened Apache NiFi in browser.

---

### Step 4

```text
Step04_Activity17.1_Process_Group.png
```

Created Process Group:

```text
Activity17.1
```

---

### Step 5

```text
Step05_ConvertExcelToCSVProcessor_Added.png
```

Added:

```text
ConvertExcelToCSVProcessor
```

---

### Step 6

```text
Step06_ConvertExcelToCSVProcessor_Properties.png
Step06_ConvertExcelToCSVProcessor_Scheduling.png
```

Configured:

```text
Sheets to Extract = Activity17_1
Run Schedule = 60 sec
```

---

### Step 7

```text
Step07_GetFile_Added.png
```

Added:

```text
GetFile
```

---

### Step 8

```text
Step08A_GetFile_Properties.png
Step08B_GetFile_Scheduling.png
```

Configured:

```text
Input Directory = /opt/nifi/nifi-current/input
File Filter = Activity17_1.xlsx
Run Schedule = 60 sec
```

---

### Step 9

```text
Step09_PutFile_Added.png
```

Added:

```text
PutFile
```

---

### Step 10

```text
Step10A_PutFile_Properties.png
Step10B_PutFile_Settings.png
```

Configured:

```text
Directory = /opt/nifi/nifi-current/output
```

and:

```text
Automatically Terminate Relationships
```

---

### Step 11

```text
Step11_GetFile_To_ConvertExcel.png
```

Connected:

```text
GetFile
    →
ConvertExcelToCSVProcessor
```

---

### Step 12

```text
Step12_ConvertExcel_To_PutFile.png
```

Connected:

```text
ConvertExcelToCSVProcessor
             →
          PutFile
```

Relationships:

```text
failure
original
success
```

---

### Step 13

```text
Step13A_GetFile_Running.png
Step13B_Input_Folder_Empty.png
```

Verified:

- GetFile running
- Input directory emptied after processing

---

### Step 14

```text
Step14_ConvertExcel_Running.png
```

Verified:

```text
ConvertExcelToCSVProcessor
```

running.

---

### Step 15

```text
Step15_PutFile_Running.png
```

Verified:

```text
PutFile
```

running.

---

### Step 16

```text
Step16_CSV_File_Created.png
```

Verified generated files in:

```text
/opt/nifi/nifi-current/output
```

Output:

```text
Activity17_1.xlsx
Activity17_1_Activity17_1.csv
```

---

## ETL Mapping

| ETL Stage | NiFi Component |
|------------|------------|
| Extract | GetFile |
| Transform | ConvertExcelToCSVProcessor |
| Load | PutFile |

---

## Troubleshooting Notes

### Issue

PutFile remained invalid and would not start.

Error:

```text
Relationship 'failure' is invalid because it is not connected
and is not auto-terminated.
```

### Resolution

Enabled:

```text
Automatically Terminate Relationships

✓ success
✓ failure
```

---

### Issue

CSV file was not initially generated.

### Root Cause

Configured:

```text
Sheets to Extract = Test
```

based on the Mini-Lesson.

Actual worksheet name:

```text
Activity17_1
```

### Resolution

Updated:

```text
Sheets to Extract = Activity17_1
```

and reprocessed the file.

CSV generation completed successfully.

---

## Results

Successfully created a complete NiFi ETL pipeline that:

- Extracted data from an Excel file
- Converted Excel data to CSV format
- Loaded the converted CSV file into the output directory

Generated Output:

```text
Activity17_1_Activity17_1.csv
```
