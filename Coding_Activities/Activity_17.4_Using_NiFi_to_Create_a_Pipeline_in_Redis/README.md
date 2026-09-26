# Required Coding Activity 17.4: Using NiFi to Create a Pipeline in Redis

## Overview

This activity demonstrates how to use Apache NiFi and Redis to create an ETL pipeline that generates the current date and time, stores the value in Redis, retrieves the value from Redis, and logs the results to the NiFi application log. The implementation uses Docker containers for both Redis and NiFi running on the same Docker network.

---

# Learning Outcome

- Use Apache NiFi to create an ETL pipeline.
- Configure Redis Controller Services.
- Store and retrieve data using Redis.
- Monitor ETL processing through NiFi logs.

---

# Environment

## Docker Containers

### NiFi

```text
Container Name: nificontainer
Image: apache/nifi:1.13.2
Port: 8080
```

### Redis

```text
Container Name: rediscontainer
Image: redis
Port: 6379
```

### Docker Network

```text
NifiNetwork
```

---

# Step 1: Start Redis

Started a Redis container and connected it to the existing Docker network.

### Command

```bash
docker run -d --name rediscontainer --network NifiNetwork -p 6379:6379 redis
```

### Verification

```bash
docker ps
```

Confirmed:

```text
rediscontainer
```

was running successfully.

### Screenshot

**Step01_Redis_Started.png**

---

# Step 2: Start NiFi

Verified that the Apache NiFi container was running successfully.

### Verification

```bash
docker ps
```

Confirmed:

```text
nificontainer
```

was running and available on port 8080.

### Screenshot

**Step02_NiFi_Started.png**

---

# Step 3: Open NiFi UI

Opened the Apache NiFi user interface.

### URL

```text
http://localhost:8080/nifi
```

Successfully accessed the NiFi canvas.

### Screenshot

**Step03_NiFi_UI.png**

---

# Step 4: Configure RedisConnectionPoolService

Opened the NiFi Flow settings and added a Controller Service.

### Controller Service

```text
RedisConnectionPoolService
```

### Properties

```text
Redis Mode
Standalone
```

```text
Connection String
rediscontainer:6379
```

> Note: The Docker hostname was used instead of localhost because Redis and NiFi were running in separate containers on the same Docker network.

### Screenshot

**Step04_RedisConnectionPoolService.png**

---

# Step 5: Configure RedisDistributedMapCacheClientService

Added a second Controller Service.

### Controller Service

```text
RedisDistributedMapCacheClientService
```

### Properties

```text
Redis Connection Pool
RedisConnectionPoolService
```

### Screenshot

**Step05_RedisDistributedMapCacheClientService.png**

---

# Step 6: Enable Controller Services

Enabled both Redis controller services.

### Enabled Services

```text
RedisConnectionPoolService
```

```text
RedisDistributedMapCacheClientService
```

Both services showed an Enabled status.

### Screenshot

**Step06_Controllers_Enabled.png**

---

# Step 7: Configure GenerateFlowFile

Added the GenerateFlowFile processor.

---

## Step 7A: Properties

Configured the processor to generate the current date and time.

### Property

```text
Custom Text
${now()}
```

### Screenshot

**Step07A_GenerateFlowFile_Properties.png**

---

## Step 7B: Scheduling

Modified the scheduling configuration.

### Scheduling

```text
Run Schedule
5 sec
```

This generated a new timestamp every five seconds.

### Screenshot

**Step07B_GenerateFlowFile_Scheduling.png**

---

# Step 8: Configure PutDistributedMapCache

Added the PutDistributedMapCache processor.

### Properties

```text
Distributed Cache Service
RedisDistributedMapCacheClientService
```

```text
Cache Entry Identifier
date
```

The processor stores the generated timestamp in Redis using the key:

```text
date
```

### Screenshot

**Step08_PutDistributedMapCache_Configured.png**

---

# Step 9: Configure FetchDistributedMapCache

Added the FetchDistributedMapCache processor.

### Properties

```text
Distributed Cache Service
RedisDistributedMapCacheClientService
```

```text
Cache Entry Identifier
date
```

```text
Put Cache Value In Attribute
date.retrieved
```

The processor retrieves the Redis value and stores it in a FlowFile attribute.

### Screenshot

**Step09_FetchDistributedMapCache_Configured.png**

---

# Step 10: Add LogAttribute

Added a LogAttribute processor.

### Processors Added

```text
GenerateFlowFile
```

```text
PutDistributedMapCache
```

```text
FetchDistributedMapCache
```

```text
LogAttribute
```

### Screenshot

**Step10_All_Processors_Created.png**

---

# Step 11: Configure and Run the Flow

Connected all processors using the success relationship.

### Flow Design

```text
GenerateFlowFile
        │
        ▼
PutDistributedMapCache
        │
        ▼
FetchDistributedMapCache
        │
        ▼
LogAttribute
```

---

## Relationships

### GenerateFlowFile

```text
success
```

### PutDistributedMapCache

```text
success
```

Automatically Terminated:

```text
failure
```

### FetchDistributedMapCache

```text
success
```

Automatically Terminated:

```text
failure
not-found
```

### LogAttribute

Automatically Terminated:

```text
success
```

---

## Results

Successfully verified:

```text
GenerateFlowFile → PutDistributedMapCache
```

```text
PutDistributedMapCache → FetchDistributedMapCache
```

```text
FetchDistributedMapCache → LogAttribute
```

FlowFiles successfully moved through every processor.

### Screenshot

**Step11_Redis_Flow_Running.png**

---

# Step 12: Verify NiFi Application Log

Opened the NiFi container and monitored the application log.

### Commands

```bash
docker exec -it nificontainer bash
```

```bash
cd /opt/nifi/nifi-current/logs
```

```bash
tail -f nifi-app.log
```

---

## Verification

The LogAttribute processor generated log entries containing:

```text
date.retrieved
```

Example:

```text
Key: 'date.retrieved'

Value: 'Sat Sep 26 08:20:21 UTC 2026'
```

Additional Redis-related attributes were logged:

```text
cached = true
```

This verified that:

1. GenerateFlowFile created a timestamp.
2. PutDistributedMapCache stored the timestamp in Redis.
3. FetchDistributedMapCache retrieved the value from Redis.
4. LogAttribute logged the retrieved value.

### Screenshot

**Step12_NiFi_App_Log.png**

---

# ETL Pipeline Architecture

```text
GenerateFlowFile
     │
     ▼
PutDistributedMapCache
(Store in Redis)
     │
     ▼
FetchDistributedMapCache
(Retrieve from Redis)
     │
     ▼
LogAttribute
(Write to nifi-app.log)
```

---

# Results

Successfully:

✅ Started Redis

✅ Started NiFi

✅ Configured RedisConnectionPoolService

✅ Configured RedisDistributedMapCacheClientService

✅ Generated timestamps using GenerateFlowFile

✅ Stored timestamps in Redis

✅ Retrieved timestamps from Redis

✅ Logged retrieved timestamps using LogAttribute

✅ Verified Redis values in the NiFi application log

---

# Conclusion

This activity demonstrated how Apache NiFi integrates with Redis to perform ETL processing. Using Redis Controller Services, the pipeline generated timestamp values, stored them in Redis, retrieved them through a distributed cache service, and logged the results to the NiFi application log. The successful execution of the flow and validation of the `date.retrieved` attribute confirmed that Redis and NiFi communicated correctly and that the ETL pipeline functioned as expected.