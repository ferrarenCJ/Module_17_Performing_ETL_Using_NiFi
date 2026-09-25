# Mini-Lesson 17.4: Using NiFi to Create a Pipeline in MongoDB

## Overview

This mini-lesson demonstrates how to use Apache NiFi to perform ETL against a MongoDB database. Data is extracted from MongoDB, transformed into FlowFiles, and written to JSON files for downstream processing.

---

# Environment Setup

## Create Docker Network

```bash
docker network create NifiNetwork
```

---

## Start NiFi Container

```bash
docker run --name nificontainer -p 8080:8080 --network NifiNetwork -d apache/nifi:1.13.2
```

---

## Create Input and Output Directories

Open the NiFi container:

```bash
docker exec -it nificontainer bash
```

Create folders:

```bash
cd /opt/nifi/nifi-current

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

## Start MongoDB Container

```bash
docker run -p 27017:27017 --name some-mongo --network NifiNetwork -d mongo
```

---

# Populate MongoDB

Open MongoDB Bash:

```bash
docker exec -it some-mongo bash
```

Start Mongo Shell:

```bash
mongosh
```

---

## Create Database and Collection

```javascript
use sample_mflix

db.movies.insertMany([
   {
      title: "Jurassic World: Fallen Kingdom",
      genres: [ "Action", "Sci-Fi" ],
      runtime: 130,
      rated: "PG-13",
      year: 2018,
      directors: [ "J. A. Bayona" ],
      cast: [ "Chris Pratt", "Bryce Dallas Howard", "Rafe Spall" ],
      type: "movie"
   },
   {
      title: "Tag",
      genres: [ "Comedy", "Action" ],
      runtime: 105,
      rated: "R",
      year: 2018,
      directors: [ "Jeff Tomsic" ],
      cast: [ "Annabelle Wallis", "Jeremy Renner", "Jon Hamm" ],
      type: "movie"
   }
])
```

Expected:

```text
acknowledged: true
insertedIds: ...
```

---

# NiFi ETL Pipeline

## Process Group

Create a process group named:

```text
Mini-Lesson 17.4
```

---

# Processor 1: GetMongo

Add:

```text
GetMongo
```

## Properties

```text
Mongo URI
mongodb://some-mongo:27017
```

```text
Mongo Database Name
sample_mflix
```

```text
Mongo Collection Name
movies
```

### Settings

Auto-terminate:

```text
failure
```

Leave:

```text
original
success
```

available for connection.

---

# Processor 2: PutFile

Add:

```text
PutFile
```

## Properties

```text
Directory
/opt/nifi/nifi-current/output
```

## Settings

Auto-terminate:

```text
failure
success
```

---

# Create Connection

Connect:

```text
GetMongo
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

# Final Pipeline

```text
GetMongo
    ↓
PutFile
```

---

# Start the Processors

Start:

```text
GetMongo
PutFile
```

---

# Verify Output File

Open NiFi Container:

```bash
docker exec -it nificontainer bash
```

Navigate:

```bash
cd /opt/nifi/nifi-current/output
```

List files:

```bash
ls -l
```

Expected:

```text
xxxxxxxx.json
```

or FlowFile-generated names.

---

# View JSON Contents

Display the file:

```bash
cat <file_name>
```

Example Output:

```json
{
  "title": "Jurassic World: Fallen Kingdom",
  "genres": [
    "Action",
    "Sci-Fi"
  ],
  "runtime": 130,
  "rated": "PG-13",
  "year": 2018,
  "directors": [
    "J. A. Bayona"
  ],
  "cast": [
    "Chris Pratt",
    "Bryce Dallas Howard",
    "Rafe Spall"
  ],
  "type": "movie"
}
```

---

# ETL Mapping

| ETL Stage | Component |
|-----------|-----------|
| Extract | GetMongo |
| Transform | NiFi FlowFile Processing |
| Load | PutFile |

---

# Data Flow

```text
MongoDB
(sample_mflix.movies)
        │
        ▼
     GetMongo
        │
        ▼
      PutFile
        │
        ▼
JSON File
(/opt/nifi/nifi-current/output)
```

---

# Key Takeaways

✅ Created a MongoDB container

✅ Created the sample_mflix database

✅ Loaded movie documents into MongoDB

✅ Configured GetMongo to read documents

✅ Configured PutFile to write JSON output

✅ Built a simple ETL pipeline in NiFi

✅ Saved MongoDB data as JSON files

✅ Verified output using Linux commands

---

# Commands Summary

## Create Network

```bash
docker network create NifiNetwork
```

## Start NiFi

```bash
docker run --name nificontainer -p 8080:8080 --network NifiNetwork -d apache/nifi:1.13.2
```

## Start MongoDB

```bash
docker run -p 27017:27017 --name some-mongo --network NifiNetwork -d mongo
```

## Enter MongoDB

```bash
docker exec -it some-mongo bash

mongosh
```

## Enter