# Mini-Lesson 17.6: Using NiFi to Create a Pipeline in Cassandra

## Overview

This mini-lesson demonstrates how to prepare the Docker environment required to build a NiFi ETL pipeline that interacts with a Cassandra database. The setup includes creating a Docker network, deploying a Cassandra container, and deploying a NiFi container connected to the same network.

---

# Prerequisites

Before creating a Cassandra-based ETL pipeline, ensure Docker Desktop is installed and running.

Required components:

- Docker Desktop
- Apache NiFi Docker image
- Cassandra Docker image

---

# Step 1: Create Docker Network

Create a shared Docker network to allow NiFi and Cassandra containers to communicate.

### Command

```bash
docker network create NifiNetwork
```

### Verify Network

```bash
docker network ls
```

Expected output should include:

```text
NifiNetwork
```

---

# Step 2: Create Cassandra Container

Create a Cassandra container and connect it to the Docker network.

### Command

```bash
docker run -p 9042:9042 --name some-cassandra --network NifiNetwork -d cassandra
```

### Parameters

```text
-p 9042:9042
```

Maps the Cassandra database port.

```text
--name some-cassandra
```

Creates the container named:

```text
some-cassandra
```

```text
--network NifiNetwork
```

Connects the container to the shared Docker network.

```text
-d
```

Runs the container in detached mode.

---

# Verify Cassandra Container

Check running containers:

```bash
docker ps
```

Expected output:

```text
some-cassandra
```

Example:

```text
CONTAINER ID   IMAGE       PORTS                    NAMES
xxxxxxxxxxxx   cassandra   0.0.0.0:9042->9042/tcp  some-cassandra
```

---

# Step 3: Create NiFi Container

Create a NiFi container connected to the same Docker network.

### Command

```bash
docker run --name nificontainer -p 8080:8080 --network NifiNetwork -d apache/nifi:1.13.2
```

### Parameters

```text
--name nificontainer
```

Names the container:

```text
nificontainer
```

```text
-p 8080:8080
```

Maps the NiFi web interface port.

```text
--network NifiNetwork
```

Connects NiFi to the same Docker network as Cassandra.

```text
-d
```

Runs NiFi in detached mode.

---

# Verify NiFi Container

Run:

```bash
docker ps
```

Expected output:

```text
nificontainer
```

Example:

```text
CONTAINER ID   IMAGE              PORTS                    NAMES
xxxxxxxxxxxx   apache/nifi:1.13.2 0.0.0.0:8080->8080/tcp  nificontainer
```

---

# Verify Both Containers

Run:

```bash
docker ps
```

Expected:

```text
some-cassandra
nificontainer
```

Both containers should display:

```text
Up
```

status.

---

# Verify Docker Network Membership

Inspect the network:

```bash
docker network inspect NifiNetwork
```

Expected members:

```text
some-cassandra
nificontainer
```

This confirms that NiFi can communicate with Cassandra internally.

---

# Open NiFi

Navigate to:

```text
http://localhost:8080/nifi
```

Expected result:

```text
NiFi UI loads successfully.
```

---

# Cassandra Connection Information

Future NiFi processors will connect using:

### Host

```text
some-cassandra
```

### Port

```text
9042
```

### Connection Example

```text
some-cassandra:9042
```

Because both containers are on the same Docker network, NiFi can resolve:

```text
some-cassandra
```

as a hostname.

---

# Architecture

```text
+----------------------+
|      Cassandra       |
|   some-cassandra     |
|      Port 9042       |
+-----------+----------+
            |
            |
     NifiNetwork
            |
            |
+-----------v----------+
|         NiFi         |
|     nificontainer    |
|      Port 8080       |
+----------------------+
```

---

# Commands Summary

## Create Network

```bash
docker network create NifiNetwork
```

## Create Cassandra Container

```bash
docker run -p 9042:9042 --name some-cassandra --network NifiNetwork -d cassandra
```

## Create NiFi Container

```bash
docker run --name nificontainer -p 8080:8080 --network NifiNetwork -d apache/nifi:1.13.2
```

## Verify Running Containers

```bash
docker ps
```

## Inspect Network

```bash
docker network inspect NifiNetwork
```

---

# Key Takeaways

✅ Created a Docker network for ETL services

✅ Deployed a Cassandra container

✅ Deployed a NiFi container

✅ Connected both containers to the same Docker network

✅ Verified communication readiness between NiFi and Cassandra

✅ Prepared the environment for Cassandra ETL workflows

---

## Recommended GitHub Location

```text
Module_17_Performing_ETL_Using_NiFi/
└── Resources/
    └── Mini_Lesson_17.6_Using_NiFi_to_Create_a_Pipeline_in_Cassandra.md
```

## Git Commands

```bash
git add .

git commit -m "Add Mini-Lesson 17.6 notes for Cassandra and NiFi setup"

git push
```