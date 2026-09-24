# Video 17.4 – NiFi System Architecture

**Duration:** 01:10

---

# Summary

This video introduces the architecture that will be used throughout Module 17 when building ETL pipelines with Apache NiFi.

The example architecture demonstrates NiFi extracting data from a MySQL database. Both MySQL and Apache NiFi run inside separate Docker containers that communicate through the same Docker network.

---

# Architecture Overview

```text
+-------------------+
| MySQL Container   |
| Source Database   |
+-------------------+
          |
          |
          ▼
+-------------------+
| Apache NiFi       |
| ETL Engine        |
+-------------------+
          |
          |
          ▼
+-------------------+
| Target System     |
| Database/Storage  |
+-------------------+