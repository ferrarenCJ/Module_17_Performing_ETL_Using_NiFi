# Mini-Lesson 17.5: Installing NiFi and Redis from Source

## Overview

This mini-lesson explains how to install Redis as a standalone application and how to install Apache NiFi locally. Unlike the previous lessons that used Docker containers, certain NiFi Redis integrations require a standalone Redis installation running directly on the operating system.

Installing software "from source" means downloading the source code, compiling the code locally, and installing the software without using a package manager or containerized deployment.

---

# What Does Installing From Source Mean?

Installing from source involves:

1. Downloading the application source code.
2. Extracting the source code files.
3. Compiling the application using a compiler.
4. Running the software directly from the compiled executable files.

Benefits include:

- Complete control over installation.
- Access to the latest source code.
- Ability to customize configuration.
- Standalone deployment independent of containers.

---

# Redis Installation

## Windows Users

The course notes indicate that Redis is no longer officially supported as a native Windows application.

### Alternative Windows Installation

1. Download:

```text
Redis-x64-3.0.504.msi
```

2. Install using the default settings.
3. Open Command Prompt.
4. Verify installation:

```cmd
redis-cli
```

If the Redis command prompt appears, the installation was successful.

---

## macOS Users

### Install Homebrew

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### Install Wget

```bash
brew install wget
```

### Verify Wget Installation

```bash
wget -V
```

Expected output:

```text
GNU Wget
```

---

# Install Redis From Source

## Step 1: Download Redis Source Code

```bash
wget http://download.redis.io/redis-stable.tar.gz
```

---

## Step 2: Extract Redis

```bash
tar xvzf redis-stable.tar.gz
```

---

## Step 3: Navigate to Redis Folder

```bash
cd redis-stable
```

---

## Step 4: Compile Redis

```bash
make
```

This command compiles the Redis source code.

---

## Step 5: Copy Configuration File

Download the Redis configuration file:

```text
redis.conf
```

Copy it into:

```text
redis-stable/src
```

---

## Step 6: Start Redis

Navigate:

```bash
cd src
```

Run:

```bash
./redis-server redis.conf
```

Expected result:

```text
Redis is starting
Ready to accept connections
```

This confirms Redis has successfully started.

---

# Installing Apache NiFi

Because Redis requires a standalone installation, a local Apache NiFi installation is also required.

---

# Windows Installation

## Step 1: Install Java

Download and install Java from Oracle.

Verify:

```cmd
java -version
```

Expected:

```text
Java version information
```

---

## Step 2: Download NiFi

Download Apache NiFi.

Example:

```text
nifi-1.15.0-bin.zip
```

---

## Step 3: Extract Files

Extract the archive to:

```text
C:\tmp\emeritus\nifi-1.15.0
```

---

# macOS Installation

## Install Java 11

```bash
brew install openjdk@11
```

Create symbolic link:

```bash
sudo ln -sfn \
/usr/local/opt/openjdk@11/libexec/openjdk.jdk \
/Library/Java/JavaVirtualMachines/openjdk-11.jdk
```

---

## Install NiFi

```bash
brew install nifi
```

---

# Starting Apache NiFi

## Windows

Navigate to:

```cmd
..\nifi-1.15.0\bin
```

Run:

```cmd
run-nifi.bat
```

---

## Retrieve Credentials

Navigate to:

```text
C:\tmp\emeritus\nifi-1.15.0\logs
```

Open:

```text
nifi-app.log
```

Search for:

```text
Generated Username
```

Example:

```text
Generated Username [960e629a-c194-445e-aaba-7986dd8c7550]

Generated Password [YOIbbTLPuRuLtkwi/1BRHdSwoOYdOrMS]
```

---

## Open NiFi UI

Navigate to:

```text
https://localhost:8443/nifi
```

Login using the generated username and password.

---

# Starting NiFi on macOS

Navigate to:

```bash
cd /usr/local/Cellar/nifi/1.15.0/libexec
```

Start NiFi:

```bash
bin/nifi.sh start
```

Expected output:

```text
NiFi started successfully
```

---

# Accessing the NiFi User Interface

Open:

```text
https://localhost:8443/nifi
```

The browser will prompt for credentials.

---

# Finding NiFi Credentials

Navigate to:

```bash
logs/
```

Open:

```text
nifi-app.log
```

Search for:

```text
username
```

Example:

```text
Generated Username [username]

Generated Password [password]
```

---

# Setting Custom Credentials

Rather than using the generated credentials, users can create their own credentials.

Navigate:

```bash
/usr/local/Cellar/nifi/1.15.0/libexec
```

Run:

```bash
bin/nifi.sh set-single-user-credentials \
<username> <password>
```

Example:

```bash
bin/nifi.sh set-single-user-credentials \
ferrarenCJ Password123
```

---

# Redis Verification

To verify Redis is running:

```bash
redis-cli ping
```

Expected response:

```text
PONG
```

---

# NiFi Verification

Verify that NiFi is running by accessing:

```text
https://localhost:8443/nifi
```

Expected result:

```text
NiFi canvas loads successfully.
```

---

# Installation Summary

## Redis

Download:

```bash
wget http://download.redis.io/redis-stable.tar.gz
```

Extract:

```bash
tar xvzf redis-stable.tar.gz
```

Compile:

```bash
make
```

Start:

```bash
./redis-server redis.conf
```

---

## NiFi

Windows:

```cmd
run-nifi.bat
```

macOS:

```bash
bin/nifi.sh start
```

---

# Architecture

```text
+----------------+
|     Redis      |
| Standalone DB  |
+-------+--------+
        |
        |
+-------v--------+
|      NiFi      |
| Local Install  |
+-------+--------+
        |
        |
+-------v--------+
|    ETL Flow    |
+----------------+
```

---

# Key Takeaways

✅ Redis can be installed directly from source code

✅ Redis requires compilation using the `make` command

✅ NiFi Redis integration requires a standalone Redis installation

✅ Apache NiFi requires Java 11

✅ NiFi automatically generates login credentials

✅ NiFi UI runs at:

```text
https://localhost:8443/nifi
```

✅ Redis can be verified using:

```bash
redis-cli ping
```

✅ Redis and NiFi can work together to support ETL workflows

---

# References

Apache NiFi Team. *Getting Started with Apache NiFi*.

Redis. *Redis Quick Start Guide*.

Java. *Download Java for Windows*.

Microsoft Archive. *Redis 3.0.504 for Windows*.