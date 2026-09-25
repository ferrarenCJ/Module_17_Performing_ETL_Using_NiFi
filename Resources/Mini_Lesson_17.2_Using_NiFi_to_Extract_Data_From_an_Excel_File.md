# Mini-Lesson 17.2 – Using NiFi to Extract Data from an Excel File

---

# Overview

Data used in ETL pipelines can originate from many different sources, including:

- CSV Files
- Excel Files
- JSON Files
- XML Files
- Relational Databases
- NoSQL Databases

Apache NiFi provides the ability to extract data from these sources, transform it into another format if needed, and load it into another system for further processing.

This mini-lesson demonstrates how to use NiFi to:

1. Read an Excel file
2. Convert the Excel file into CSV format
3. Store the converted output file
4. Build a simple ETL workflow using FlowFiles, Processors, and Connectors

---

# Learning Objectives

By the end of this lesson, you should be able to:

- Create a Docker network
- Deploy a NiFi container
- Create input and output folders
- Copy files into a Docker container
- Create a Process Group
- Configure GetFile processors
- Configure ConvertExcelToCSVProcessor processors
- Configure PutFile processors
- Connect processors using connectors
- Execute a NiFi workflow
- Convert Excel files into CSV files

---

# ETL Architecture

```text
Excel File
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
CSV Output File
```

---

# Step 1: Create a Docker Network

Create a dedicated network for NiFi.

```bash
docker network create NifiNetwork
```

Purpose:

- Enables communication between containers
- Simplifies container management
- Supports future database integrations

Verify:

```bash
docker network ls
```

---

# Step 2: Create the NiFi Container

Create the Apache NiFi container and connect it to the NifiNetwork.

```bash
docker run --name nificontainer \
-p 8080:8080 \
--network NifiNetwork \
-d apache/nifi:1.13.2
```

---

# Parameters Explained

## --name

Container name:

```text
nificontainer
```

---

## -p 8080:8080

Maps container port to local machine.

```text
Localhost:8080
        │
        ▼
Container:8080
```

---

## --network

Associates container with:

```text
NifiNetwork
```

---

## -d

Runs container in detached mode.

---

# Expected Environment

```text
Docker Host
│
└── nificontainer
        │
        ▼
Apache NiFi
```

---

# Step 3: Create Input and Output Folders

Open a terminal inside the NiFi container.

Navigate to:

```bash
cd /opt/nifi/nifi-current
```

Create folders:

```bash
mkdir input
mkdir output
```

---

# Purpose of the Directories

## Input Folder

```text
/opt/nifi/nifi-current/input
```

Stores:

- Incoming Excel files

---

## Output Folder

```text
/opt/nifi/nifi-current/output
```

Stores:

- Converted CSV files
- Processed output files

---

# Directory Structure

```text
nifi-current
│
├── input
│
└── output
```

---

# Step 4: Copy the Excel File

Copy the provided lesson file from your local machine into the container.

```bash
docker cp Activity17-2.xlsx \
nificontainer:/opt/nifi/nifi-current/input
```

---

# Result

```text
Local Machine
      │
      ▼
Activity17-2.xlsx
      │
      ▼
NiFi Input Folder
```

---

# Step 5: Launch NiFi

Navigate to:

```text
http://localhost:8080/nifi/
```

---

# Access Options

Through Docker Desktop:

```text
Container
    ↓
Open CLI
    ↓
Open Browser
```

---

# NiFi Canvas

The NiFi canvas is used to visually build workflows.

Example:

```text
Processor
     ↓
Processor
     ↓
Processor
```

---

# Step 6: Create a Process Group

A Process Group is used to organize related workflows.

Drag a Process Group onto the canvas.

Name:

```text
Mini-Lesson 17.2 - Excel
```

Select:

```text
Add
```

---

# Why Use Process Groups?

Benefits:

- Organization
- Reusability
- Easier maintenance

All workflows inside the group remain logically connected.

---

# Step 7: Add the GetFile Processor

Enter the Process Group.

Drag a Processor onto the blank canvas.

Select:

```text
GetFile
```

---

# Purpose

GetFile monitors a directory and retrieves files.

Input:

```text
Excel File
```

Output:

```text
FlowFile
```

---

# Configure GetFile

## Scheduling Tab

Set:

```text
Run Schedule = 60 sec
```

---

## Properties Tab

### Input Directory

```text
/opt/nifi/nifi-current/input
```

### File Filter

```text
Activity17_2.xlsx
```

---

# GetFile Workflow

```text
Input Folder
      │
      ▼
GetFile
      │
      ▼
FlowFile
```

---

# Step 8: Add ConvertExcelToCSVProcessor

Add another processor.

Processor Type:

```text
ConvertExcelToCSVProcessor
```

---

# Purpose

Convert:

```text
Excel
    ↓
CSV
```

---

# Configure ConvertExcelToCSVProcessor

## Scheduling Tab

```text
Run Schedule = 60 sec
```

---

## Properties Tab

### Sheets to Extract

```text
Test
```

---

# Workflow

```text
Excel
     │
     ▼
ConvertExcelToCSVProcessor
     │
     ▼
CSV
```

---

# Step 9: Connect Processors

Connect:

```text
GetFile
      │
      ▼
ConvertExcelToCSVProcessor
```

This connector transfers the FlowFile between processors.

---

# Step 10: Add PutFile Processor

Add another processor.

Type:

```text
PutFile
```

---

# Purpose

Write the converted file to disk.

Destination:

```text
/opt/nifi/nifi-current/output
```

---

# Configure PutFile

## Scheduling Tab

```text
Run Schedule = 60 sec
```

## Properties Tab

Directory:

```text
/opt/nifi/nifi-current/output
```

---

# Step 11: Connect Processors

Create connector:

```text
ConvertExcelToCSVProcessor
            │
            ▼
        PutFile
```

---

# Relationship Configuration

Select:

```text
failure
original
success
```

This ensures all outcomes are routed.

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

# ETL Interpretation

## Extract

Performed by:

```text
GetFile
```

Reads Excel file.

---

## Transform

Performed by:

```text
ConvertExcelToCSVProcessor
```

Converts:

```text
.xlsx
    ↓
.csv
```

---

## Load

Performed by:

```text
PutFile
```

Stores output file.

---

# Complete ETL Flow

```text
Excel File
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
CSV File
```

---

# Execute the Workflow

Start:

```text
GetFile
ConvertExcelToCSVProcessor
PutFile
```

---

# Clearing Queues

If files remain in queues:

```text
Right Click Background
        ↓
Empty All Queues
```

This ensures a clean run.

---

# Expected Results

Input Folder

```text
Activity17_2.xlsx
```

↓

Output Folder

```text
Activity17_2.xlsx
MiniLesson17_2_Test.csv
```

---

# Generated Files

## Original File Copy

```text
Activity17_2.xlsx
```

---

## Converted File

```text
MiniLesson17_2_Test.csv
```

Naming Convention:

```text
Original Filename
        +
Sheet Name
```

---

# FlowFile Lifecycle

```text
Excel File
      │
      ▼
Create FlowFile
      │
      ▼
Read File
      │
      ▼
Transform
      │
      ▼
Write CSV
```

---

# Components Used

## Process Group

```text
Mini-Lesson 17.2 - Excel
```

---

## Processors

### GetFile

Extract data.

### ConvertExcelToCSVProcessor

Transform data.

### PutFile

Load data.

---

## Connectors

Move FlowFiles between processors.

---

## FlowFiles

Contain:

```text
Content
+
Attributes
```

---

# Key Takeaways

✅ NiFi can extract data directly from Excel files

✅ GetFile retrieves files from a directory

✅ ConvertExcelToCSVProcessor converts Excel to CSV

✅ PutFile stores processed output

✅ Process Groups organize workflows

✅ Connectors move FlowFiles between processors

✅ This lesson demonstrates all three stages of ETL

---

# ETL Mapping

| ETL Stage | NiFi Component |
|------------|---------------|
| Extract | GetFile |
| Transform | ConvertExcelToCSVProcessor |
| Load | PutFile |

---

# Exam Notes

✅ Create Docker network before creating containers

✅ NiFi listens on port 8080

✅ Process Groups organize workflows

✅ GetFile reads files

✅ ConvertExcelToCSVProcessor transforms data

✅ PutFile writes output

✅ FlowFiles move through the pipeline

✅ Connectors link processors

✅ Excel files can be transformed into CSV files

✅ This workflow demonstrates a complete ETL pipeline

---

# Personal Notes

This is the first fully implemented ETL pipeline in Module 17.

The workflow is intentionally simple:

```text
Excel
   ↓
CSV
```

However, it introduces the same architecture that will be used later for:

- MySQL
- MongoDB
- Redis
- Cassandra

The most important concept is understanding how processors are chained together through connectors to create an automated ETL workflow.