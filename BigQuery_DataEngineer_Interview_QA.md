# ☁️ Google BigQuery Interview Questions & Answers
### JSW Data Engineer Interview Preparation — Friday

---
## 📋 Table of Contents
> Click on any question to jump directly to its answer

- [Q1. What is Google BigQuery?](#q1)
- [Q2. What is BigQuery Architecture? (Dremel, Colossus, Jupiter, Borg)](#q2)
- [Q3. What is the difference between BigQuery and Traditional Database?](#q3)
- [Q4. What is Columnar Storage in BigQuery?](#q4)
- [Q5. What is the difference between Storage and Compute in BigQuery?](#q5)
- [Q6. What are BigQuery Slots?](#q6)
- [Q7. What are the different BigQuery Pricing Models?](#q7)
- [Q8. What are Datasets, Tables and Views in BigQuery?](#q8)
- [Q9. What are the different Table Types in BigQuery?](#q9)
- [Q10. What are the different ways to load data into BigQuery?](#q10)
- [Q11. What file formats does BigQuery support for data loading?](#q11)
- [Q12. What is Partitioning in BigQuery?](#q12)
- [Q13. What are the types of Partitioning in BigQuery?](#q13)
- [Q14. What is Clustering in BigQuery?](#q14)
- [Q15. What is the difference between Partitioning and Clustering?](#q15)
- [Q16. How does BigQuery charge for queries? Does LIMIT reduce cost?](#q16)
- [Q17. What is Query Optimization in BigQuery?](#q17)
- [Q18. What is a Materialized View in BigQuery?](#q18)
- [Q19. What is the difference between View and Materialized View?](#q19)
- [Q20. What is BigQuery ML?](#q20)
- [Q21. What is Streaming Ingestion in BigQuery?](#q21)
- [Q22. What is the difference between Batch and Streaming in BigQuery?](#q22)
- [Q23. What are External Tables / Federated Queries in BigQuery?](#q23)
- [Q24. What is BigQuery Data Transfer Service?](#q24)
- [Q25. How does BigQuery handle Security?](#q25)
- [Q26. What are IAM Roles in BigQuery?](#q26)
- [Q27. What is a BigQuery Job?](#q27)
- [Q28. What is a Dry Run in BigQuery?](#q28)
- [Q29. What is Query Caching in BigQuery?](#q29)
- [Q30. What are the Advantages and Disadvantages of BigQuery?](#q30)
- [Q31. What is the difference between BigQuery and Cloud SQL?](#q31)
- [Q32. What is BigQuery INFORMATION_SCHEMA?](#q32)
- [Q33. How do you connect Python to BigQuery?](#q33)
- [Q34. How do you write a BigQuery SQL query with Partitioned tables?](#q34)
- [Q35. What is ARRAY and STRUCT in BigQuery?](#q35)
- [Q36. What is UNNEST in BigQuery?](#q36)
- [Q37. How do you handle duplicate data in BigQuery?](#q37)
- [Q38. What is the Table Snapshot and Table Clone in BigQuery?](#q38)
- [Q39. What is BigQuery Omni?](#q39)
- [Q40. How do you monitor and debug BigQuery jobs?](#q40)
- [Q41. BigQuery vs Snowflake vs Redshift — Key Differences](#q41)
- [Q42. What is Data Lake vs Data Warehouse vs Data Lakehouse?](#q42)
- [Q43. What is Google Cloud Pub/Sub and how does it work with BigQuery?](#q43)
- [Q44. What is Google Cloud Dataflow?](#q44)
- [Q45. What are Common BigQuery SQL Functions for Data Engineers?](#q45)
- [Q46. What are Window Functions in BigQuery?](#q46)
- [Q47. What is Table Expiration and Partition Expiration in BigQuery?](#q47)
- [Q48. What is BigQuery Scheduled Queries?](#q48)
- [Q49. How do you implement SCD Type 2 in BigQuery?](#q49)
- [Q50. What are BigQuery Best Practices for Data Engineers?](#q50)
- [Q51. Standard SQL vs Legacy SQL in BigQuery?](#q51)
- [Q52. What is BigQuery's MERGE statement?](#q52)
- [Q53. How do you load data from GCS to BigQuery using Python?](#q53)
- [Q54. What is BigQuery's Time Travel feature?](#q54)
- [Q55. What is a Real-World Data Pipeline using BigQuery?](#q55)

---

## 🔵 SECTION 1: BIGQUERY BASICS (Q1–Q11)

---

<a id="q1"></a>
### 🔷 Q1. What is Google BigQuery?

> **Definition:**
> - Google BigQuery is a fully managed, serverless, cloud-native data warehouse on Google Cloud Platform (GCP)
> - Designed for large-scale data analytics — can process terabytes in seconds, petabytes in minutes
> - No server setup, no infrastructure management required
> - Uses standard SQL — easy to use if you know SQL
> - Owned by Google (part of GCP since 2010, originally powered by Dremel engine used internally at Google since 2006)
>
> **Key Characteristics:**
> - **Serverless** — no infrastructure to manage
> - **Fully managed** — Google handles everything (storage, scaling, backups)
> - **OLAP system** — designed for analytical queries, not transactions
> - **Columnar storage** — reads only columns needed, very fast
> - **Pay-as-you-go** — pay only for data scanned and storage used
>
> **Common Use Cases:**
> - Large-scale data analytics and reporting
> - Business intelligence dashboards (connects with Looker, Data Studio)
> - Log analysis — process millions of events per second
> - Real-time analytics with streaming ingestion
> - Machine learning with BigQuery ML
> - E-commerce, healthcare, finance analytics

---

<a id="q2"></a>
### 🔷 Q2. What is BigQuery Architecture? (Dremel, Colossus, Jupiter, Borg)

> **BigQuery is built on 4 core Google technologies:**
>
> **1. Dremel — Query Engine (Compute):**
> - Massively parallel processing (MPP) query execution engine
> - Breaks SQL queries into smaller tasks and distributes across thousands of workers simultaneously
> - Uses a tree-based execution model — root node → intermediate nodes → leaf nodes
> - Leaf nodes read actual data, intermediate nodes aggregate results, root node returns final result
> - This is why BigQuery can scan terabytes in seconds — thousands of workers run in parallel
>
> **2. Colossus — Storage:**
> - Google's global distributed file system (next generation of GFS)
> - Stores BigQuery data in columnar format called Capacitor
> - Data is replicated across multiple data centers for durability
> - Automatically handles disk or node failures — self-healing storage
>
> **3. Jupiter — Network:**
> - Google's internal petabit-scale network
> - Connects compute (Dremel) and storage (Colossus) at ultra-high speed
> - Provides 1 Petabit/sec of total bandwidth
> - Enables fast data transfer between storage and compute
>
> **4. Borg — Cluster Management:**
> - Google's internal cluster management system (precursor to Kubernetes)
> - Allocates and manages compute resources for Dremel jobs
> - Handles fault tolerance and job scheduling
>
> **How a query flows:**
> - User submits SQL → Dremel parses and optimizes → Broken into stages → Slots assigned to workers → Workers read columns from Colossus → Results aggregated through tree → Final result returned to user

---

<a id="q3"></a>
### 🔷 Q3. What is the difference between BigQuery and Traditional Database?

| Feature | BigQuery | Traditional Database (PostgreSQL/MySQL) |
|---|---|---|
| Architecture | Serverless, cloud-native | Server-based, needs installation |
| Storage | Columnar (Capacitor) | Row-based |
| Scaling | Auto-scales to petabytes | Manual scaling, hardware limited |
| Transactions (OLTP) | Not ideal (limited INSERT/UPDATE) | Excellent — designed for it |
| Analytics (OLAP) | Excellent — designed for it | Slower on large datasets |
| Query Speed | Seconds on terabytes | Minutes/hours on same data |
| Cost | Pay per byte scanned + storage | Fixed infrastructure cost |
| Index | No traditional indexes | Uses indexes, B-trees |
| Setup | Zero setup — log in and start | Requires installation, admin |
| SQL | Standard SQL + BigQuery extensions | Standard SQL |

---

<a id="q4"></a>
### 🔷 Q4. What is Columnar Storage in BigQuery?

> **What it means:**
> - Traditional databases store data row by row — to read one column, you read all columns of every row
> - BigQuery stores data column by column — to read one column, only that column's data is read
>
> **Example:**
> - Table has 10 columns, 1 billion rows
> - Query: `SELECT SUM(sales) FROM orders`
> - Row storage: reads all 10 columns × 1 billion rows = massive I/O
> - Column storage: reads only `sales` column = 10x less data read
>
> **Benefits:**
> - Much faster for analytical queries that touch only a few columns
> - Better compression — same type of data in one column compresses well
> - Lower query cost — BigQuery charges per bytes read
> - Typical compression ratio: 4 to 10 times better than raw CSV
>
> **Practical tip:**
> - Always use `SELECT column1, column2` instead of `SELECT *`
> - `SELECT *` reads all columns — wastes money and time

---

<a id="q5"></a>
### 🔷 Q5. What is the difference between Storage and Compute in BigQuery?

> **Separation of Storage and Compute is BigQuery's key design principle:**
>
> **Storage (Colossus):**
> - Where your data lives — tables, partitions, columns
> - Stores data in Capacitor columnar format
> - Scales independently — can store petabytes cheaply
> - Charged per TB per month
>
> **Compute (Dremel/Slots):**
> - Where queries are executed
> - Uses slots (virtual CPUs) to process queries
> - Scales independently — more slots = faster queries
> - Charged per bytes processed (on-demand) or per reserved slots (flat-rate)
>
> **Why this separation matters:**
> - Scale storage without paying for idle compute
> - Scale compute without affecting storage
> - Pay only when running queries — no idle compute costs
> - Multi-region storage without impacting query speed
> - More flexible and cost-efficient than traditional systems

---

<a id="q6"></a>
### 🔷 Q6. What are BigQuery Slots?

> **Definition:**
> - A slot is a unit of computational capacity in BigQuery
> - Each slot represents a certain amount of CPU, memory and networking resources
> - Slots are the building blocks of BigQuery's distributed query execution
>
> **How slots work:**
> - When you run a query, BigQuery breaks it into tasks
> - Each task is assigned to one or more slots to run in parallel
> - More slots = more parallelism = faster query execution
> - Number of slots auto-determined by query complexity and data size
>
> **Two types of slot pricing:**
>
> | | On-Demand Slots | Reserved Slots (Flat-Rate) |
> |---|---|---|
> | How | Shared pool, auto-allocated | You reserve a fixed number |
> | Cost | Pay per bytes processed | Fixed monthly cost |
> | Max slots | 2000 concurrent per project | Whatever you reserve |
> | Best for | Variable/unpredictable workloads | Heavy, predictable workloads |
>
> **Slot reservations:**
> - Reserve a specific number of slots for guaranteed performance
> - Ensures queries don't compete for resources with other projects
> - More predictable costs and query times

---

<a id="q7"></a>
### 🔷 Q7. What are the different BigQuery Pricing Models?

> **1. On-Demand Pricing:**
> - Pay for what you query — charged per bytes scanned
> - First 1 TB per month is free
> - After that, approximately $5 per TB scanned
> - Good for unpredictable or low-volume workloads
>
> **2. Flat-Rate (Capacity) Pricing:**
> - Reserve a fixed number of slots at a monthly cost
> - Predictable costs regardless of how much data you query
> - Good for heavy users with consistent workloads
>
> **3. Storage Pricing:**
> - Active storage (modified in last 90 days) — charged per TB/month
> - Long-term storage (not modified in 90+ days) — cheaper rate automatically
> - First 10 GB per month is free
>
> **Cost Reduction Tips:**
> - Use `SELECT column1, column2` instead of `SELECT *`
> - Use partitioning — query only relevant partitions
> - Use clustering — skip irrelevant data blocks
> - Use Dry Run before running — see estimated cost
> - LIMIT clause does NOT reduce cost — BigQuery scans all data first
> - Use materialized views for repeated aggregation queries

---

<a id="q8"></a>
### 🔷 Q8. What are Datasets, Tables and Views in BigQuery?

> **Dataset:**
> - Top-level container that organizes and controls access to data
> - Similar to a database or schema in SQL
> - Contains tables, views and other BigQuery objects
> - Has a geographic location (region) — data stays in that region
>
> **Table:**
> - Collection of rows and columns with a defined schema
> - Actual data is stored here in Colossus
> - Types: native tables, external tables, partitioned tables, clustered tables
>
> **View:**
> - Virtual table defined by a SQL query
> - Does not store data — runs the underlying query each time
> - Used for: reusable queries, security (hide sensitive columns), simplification
>
> **Materialized View:**
> - View that pre-computes and stores the query result
> - Much faster than regular views for repeated queries
> - Needs to be refreshed when underlying data changes
>
> **BigQuery structure hierarchy:**
> ```
> GCP Project
> └── Dataset
>     ├── Table 1
>     ├── Table 2
>     ├── View
>     └── Materialized View
> ```

---

<a id="q9"></a>
### 🔷 Q9. What are the different Table Types in BigQuery?

> **1. Native Tables:**
> - Data stored directly in BigQuery's Colossus storage
> - Fastest performance
> - Supports partitioning and clustering
>
> **2. External Tables:**
> - Data stored outside BigQuery (Google Cloud Storage, Google Sheets, Bigtable)
> - Query data in-place without loading into BigQuery
> - Slower than native tables — data is read from external source each time
>
> **3. Partitioned Tables:**
> - Table divided into smaller segments based on a column
> - Queries scan only relevant partitions — faster and cheaper
>
> **4. Clustered Tables:**
> - Data organized within partitions by column values
> - Related rows stored close together
> - Speeds up filter and aggregation queries
>
> **5. Materialized Views:**
> - Pre-computed view results stored as a table
> - Auto-refreshed when base table changes
>
> **6. Temporary Tables:**
> - Exist only during a session or query execution
> - Automatically deleted after 24 hours
> - Used for intermediate processing
>
> **7. Table Snapshots:**
> - Point-in-time read-only copy of a table
> - Useful for backup and audit purposes
>
> **8. Cloned Tables:**
> - Writable copy of a table without duplicating storage
> - Only stores the differences (delta) — cost-efficient

---

<a id="q10"></a>
### 🔷 Q10. What are the different ways to load data into BigQuery?

> **1. Batch Loading:**
> - Upload files from local machine or Google Cloud Storage (GCS)
> - Supported formats: CSV, JSON, Avro, Parquet, ORC
> - Free — no charge for batch loading
> - Best for: large historical data loads, one-time migration
>
> **2. Streaming Ingestion:**
> - Insert rows in real-time using BigQuery Storage Write API or streaming API
> - Data available for querying within seconds
> - Charged per GB of data streamed
> - Best for: real-time dashboards, event tracking, IoT data
>
> **3. BigQuery Data Transfer Service:**
> - Automated scheduled data transfers from Google services
> - Sources: Google Analytics, Google Ads, YouTube, S3, Teradata
> - Set up once — runs automatically on schedule
>
> **4. Cloud Dataflow:**
> - Apache Beam-based fully managed pipeline service
> - For complex ETL — transform data before loading
> - Handles both batch and streaming data
>
> **5. Cloud Data Fusion:**
> - Visual drag-and-drop ETL tool
> - Build pipelines without coding
> - Connects to 150+ data sources
>
> **6. federated Queries:**
> - Query data directly from GCS, Cloud SQL, Bigtable without loading
> - No ingestion needed — data stays in original source
>
> **7. BigQuery Storage API (Python/Java):**
> - Programmatic bulk loading using Python or Java clients
> - Best for custom pipelines

---

<a id="q11"></a>
### 🔷 Q11. What file formats does BigQuery support for data loading?

| Format | Best For | Notes |
|---|---|---|
| CSV | Simple tabular data | Easy but larger file size |
| JSON | Nested/semi-structured data | Supports arrays and structs |
| Avro | Large batch loads | Schema embedded in file, best for streaming |
| Parquet | Analytical workloads | Columnar format — fastest load, best compression |
| ORC | Hadoop ecosystem data | Columnar, good compression |

> **Recommendation for Data Engineers:**
> - Use **Parquet** for best performance — columnar format aligns with BigQuery's storage
> - Use **Avro** for streaming loads — schema is self-describing
> - Avoid CSV for large datasets — no schema enforcement, larger file size

---

## 🟡 SECTION 2: PARTITIONING & CLUSTERING (Q12–Q16)

---

<a id="q12"></a>
### 🔷 Q12. What is Partitioning in BigQuery?

> **Definition:**
> - Partitioning divides a large table into smaller segments called partitions based on a column value
> - When you query with a filter on the partition column, BigQuery only scans the relevant partitions
> - The rest of the data is skipped — called **partition pruning**
>
> **Why it matters:**
> - Reduces bytes scanned → reduces query cost
> - Reduces query time — less data to process
> - Easier data lifecycle management — delete old partitions easily
>
> **Example:**
> - Table has 5 years of order data = 1 TB total
> - Partitioned by `order_date`
> - Query for only January 2024 data → scans only January partition (~17 GB)
> - Without partition → scans entire 1 TB = 60x more expensive
>
> ```sql
> -- Create partitioned table
> CREATE TABLE project.dataset.orders
> PARTITION BY DATE(order_date)
> AS SELECT * FROM project.dataset.raw_orders;
>
> -- Query uses partition pruning automatically
> SELECT region, SUM(amount)
> FROM project.dataset.orders
> WHERE order_date BETWEEN '2024-01-01' AND '2024-01-31'
> GROUP BY region;
> ```

---

<a id="q13"></a>
### 🔷 Q13. What are the types of Partitioning in BigQuery?

> **1. Time-unit Column Partitioning (most common):**
> - Partition by a DATE, DATETIME or TIMESTAMP column
> - Choose granularity: HOUR, DAY, MONTH, YEAR
> - Best for: time-series data, event logs, transaction data
>
> **2. Ingestion-time Partitioning:**
> - BigQuery automatically creates partitions based on when data was loaded
> - Uses `_PARTITIONTIME` or `_PARTITIONDATE` pseudo-columns
> - Best for: streaming data, append-only pipelines
>
> **3. Integer Range Partitioning:**
> - Partition by an integer column with a defined range and interval
> - Example: partition by `customer_id` in ranges 0-999, 1000-1999, etc.
> - Best for: non-time-based segmentation
>
> ```sql
> -- Time partitioning
> CREATE TABLE sales
> PARTITION BY DATE(transaction_date)
> OPTIONS (partition_expiration_days = 365);
>
> -- Integer range partitioning
> CREATE TABLE customers
> PARTITION BY RANGE_BUCKET(customer_id, GENERATE_ARRAY(0, 100, 10));
>
> -- Ingestion-time partitioning
> CREATE TABLE events
> PARTITION BY _PARTITIONDATE;
> ```

---

<a id="q14"></a>
### 🔷 Q14. What is Clustering in BigQuery?

> **Definition:**
> - Clustering organizes data within a table or partition based on the values of one or more columns
> - Related rows are stored close together on disk — called co-location
> - When you filter on a clustered column, BigQuery can skip entire blocks of data — called **block pruning**
>
> **How it works:**
> - You specify 1 to 4 clustering columns when creating the table
> - BigQuery sorts data by those columns and stores related rows together
> - Queries filtering on clustered columns scan less data automatically
>
> **Why it matters:**
> - Faster queries — less data scanned per query
> - Lower cost — fewer bytes read
> - Especially useful for high-cardinality columns (many unique values)
>
> ```sql
> -- Create clustered table
> CREATE TABLE project.dataset.sales
> PARTITION BY DATE(order_date)
> CLUSTER BY region, product_category
> AS SELECT * FROM raw_sales;
>
> -- This query benefits from both partitioning AND clustering
> SELECT SUM(amount)
> FROM project.dataset.sales
> WHERE order_date = '2024-01-15'   -- partition pruning
>   AND region = 'East'              -- cluster block pruning
>   AND product_category = 'Electronics';  -- cluster block pruning
> ```

---

<a id="q15"></a>
### 🔷 Q15. What is the difference between Partitioning and Clustering?

| | Partitioning | Clustering |
|---|---|---|
| How | Divides table into separate physical segments | Organizes rows within a table/partition by column values |
| Data skipping | Skips entire partitions | Skips blocks of data within partitions |
| Column types | DATE, TIMESTAMP, INTEGER only | Any column type (up to 4 columns) |
| Cost estimate | Shown in dry run before query | Shown only after query runs |
| When to use | Time-based or range-based filtering | Multi-column filtering and aggregation |
| Partition limit | Max 4000 partitions per table | No limit |

> **Best Practice — Use BOTH together:**
> - Partition by date — for time-based filtering
> - Cluster by region, category — for further narrowing
> - Example: daily orders table partitioned by date, clustered by region and product

---

<a id="q16"></a>
### 🔷 Q16. How does BigQuery charge for queries? Does LIMIT reduce cost?

> **How BigQuery charges:**
> - Charges based on **bytes scanned** (data read) during query execution
> - NOT based on rows returned, number of tables, or query execution time
> - On-demand: ~$5 per TB scanned (first 1 TB free per month)
>
> **Does LIMIT reduce cost? ❌ NO**
> - `LIMIT 10` does NOT reduce cost
> - BigQuery scans all relevant data first, then returns only the limited rows
> - You still pay for all bytes scanned
>
> ```sql
> -- ❌ LIMIT does NOT save money
> SELECT * FROM orders LIMIT 10;  -- still scans full table
>
> -- ✅ SELECT only needed columns — saves money
> SELECT order_id, amount FROM orders;
>
> -- ✅ Filter on partition column — saves money
> SELECT * FROM orders WHERE order_date = '2024-01-15';
>
> -- ✅ Use Dry Run to check cost before running
> ```
>
> **What DOES reduce cost:**
> - Select specific columns instead of `SELECT *`
> - Filter on partitioned columns → partition pruning
> - Filter on clustered columns → block pruning
> - Use materialized views for repeated queries
> - Use cached results — repeated identical queries are free

---

## 🟠 SECTION 3: QUERY OPTIMIZATION & ADVANCED (Q17–Q29)

---

<a id="q17"></a>
### 🔷 Q17. What is Query Optimization in BigQuery?

> **Key optimization techniques:**
>
> **1. Select only needed columns:**
> ```sql
> -- ❌ Bad — reads all columns
> SELECT * FROM orders;
>
> -- ✅ Good — reads only 2 columns
> SELECT order_id, amount FROM orders;
> ```
>
> **2. Filter on partition column:**
> ```sql
> -- ✅ Partition pruning — only scans Jan 2024 data
> SELECT * FROM orders
> WHERE order_date BETWEEN '2024-01-01' AND '2024-01-31';
> ```
>
> **3. Avoid functions on partition columns:**
> ```sql
> -- ❌ Bad — function prevents partition pruning
> WHERE EXTRACT(YEAR FROM order_date) = 2024
>
> -- ✅ Good — direct range filter uses partition pruning
> WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31'
> ```
>
> **4. Reduce data before JOINs:**
> ```sql
> -- ✅ Filter inside subquery before joining
> SELECT a.id, b.amount
> FROM table_a a
> JOIN (SELECT id, amount FROM table_b WHERE region = 'East') b
> ON a.id = b.id;
> ```
>
> **5. Use Materialized Views for repeated queries**
>
> **6. Use Dry Run before executing:**
> - Check estimated bytes scanned before running
> - Prevents accidental expensive queries
>
> **7. Use EXPLAIN / Query Plan:**
> - Check which stages take most time
> - Identify data skew and bottlenecks

---

<a id="q18"></a>
### 🔷 Q18. What is a Materialized View in BigQuery?

> **Definition:**
> - A materialized view is a pre-computed view that stores the query result as a physical table
> - Unlike regular views (which run the query every time), materialized views store the result
> - BigQuery automatically updates materialized views when base tables change
>
> **Benefits:**
> - Much faster for repeated aggregation queries — result already computed
> - Reduces query cost — fewer bytes scanned
> - Auto-refreshed by BigQuery — no manual maintenance needed
> - Especially useful for dashboard queries that run repeatedly
>
> ```sql
> -- Create materialized view
> CREATE MATERIALIZED VIEW project.dataset.monthly_sales AS
> SELECT
>     DATE_TRUNC(order_date, MONTH) as month,
>     region,
>     SUM(amount) as total_sales,
>     COUNT(*) as order_count
> FROM project.dataset.orders
> GROUP BY 1, 2;
>
> -- Query runs in milliseconds instead of seconds
> SELECT * FROM project.dataset.monthly_sales
> WHERE month = '2024-01-01';
> ```

---

<a id="q19"></a>
### 🔷 Q19. What is the difference between View and Materialized View?

| | View | Materialized View |
|---|---|---|
| Stores data | ❌ No — runs query every time | ✅ Yes — stores pre-computed result |
| Query speed | Slower — recalculated each time | Faster — reads stored result |
| Data freshness | Always fresh (real-time) | Auto-refreshed but may have small delay |
| Storage cost | ❌ None | ✅ Charges for stored data |
| Best for | Simple, infrequent queries | Complex, repeated aggregations |
| Auto-refresh | N/A | ✅ Yes — BigQuery handles it |

---

<a id="q20"></a>
### 🔷 Q20. What is BigQuery ML?

> **Definition:**
> - BigQuery ML allows you to create and run machine learning models directly inside BigQuery using SQL
> - No need to export data to Python or other ML tools — ML runs where data lives
>
> **Supported model types:**
> - Linear Regression — predict numeric values (sales forecasting)
> - Logistic Regression — classification (churn prediction)
> - K-Means Clustering — customer segmentation
> - Matrix Factorization — recommendation systems
> - Time-series forecasting — ARIMA models
> - Deep Neural Networks — complex pattern recognition
> - Import TensorFlow models — use custom trained models
>
> **How it works:**
> ```sql
> -- Step 1: Create and train a model
> CREATE OR REPLACE MODEL project.dataset.sales_model
> OPTIONS (model_type = 'linear_reg', input_label_cols = ['amount'])
> AS SELECT region, product_category, discount, amount
>    FROM project.dataset.orders;
>
> -- Step 2: Evaluate the model
> SELECT * FROM ML.EVALUATE(MODEL project.dataset.sales_model);
>
> -- Step 3: Make predictions
> SELECT * FROM ML.PREDICT(
>     MODEL project.dataset.sales_model,
>     (SELECT region, product_category, discount FROM new_orders)
> );
> ```
>
> **Benefits:**
> - No data movement — train where data lives
> - No separate ML infrastructure to manage
> - Use SQL — no Python/R needed for basic models
> - Cost-efficient for simple models

---

<a id="q21"></a>
### 🔷 Q21. What is Streaming Ingestion in BigQuery?

> **Definition:**
> - Streaming ingestion allows you to insert rows into BigQuery tables in real-time
> - Data becomes available for querying within seconds of insertion
> - Used for real-time analytics where data cannot wait for batch loads
>
> **How to stream data:**
> ```python
> from google.cloud import bigquery
>
> client = bigquery.Client()
> table_id = "project.dataset.events"
>
> # Stream rows
> rows_to_insert = [
>     {"event": "click", "user_id": 101, "timestamp": "2024-01-15T10:30:00"},
>     {"event": "purchase", "user_id": 102, "timestamp": "2024-01-15T10:31:00"},
> ]
>
> errors = client.insert_rows_json(table_id, rows_to_insert)
> if not errors:
>     print("Rows streamed successfully")
> ```
>
> **Key points:**
> - Charged per GB of data streamed (more expensive than batch)
> - Streamed data available for querying in seconds
> - Streamed rows cannot be deleted or updated immediately
> - Best for: real-time dashboards, event tracking, IoT sensor data

---

<a id="q22"></a>
### 🔷 Q22. What is the difference between Batch and Streaming in BigQuery?

| | Batch Loading | Streaming Ingestion |
|---|---|---|
| Speed | Loads large files at once | Inserts rows one at a time in real-time |
| Latency | Minutes to hours | Seconds |
| Cost | Free to load | Charged per GB streamed |
| Use case | Historical data, one-time migration | Real-time events, IoT data, logs |
| Availability | Available after load job | Available within seconds |
| Format | Files (CSV, Parquet, JSON) | JSON rows via API |
| Update/Delete | Possible after load | Not immediately possible |

---

<a id="q23"></a>
### 🔷 Q23. What are External Tables / Federated Queries in BigQuery?

> **External Tables:**
> - Tables that point to data stored OUTSIDE BigQuery (in GCS, Google Sheets, Bigtable, Cloud SQL)
> - Data is NOT loaded into BigQuery — queried in place
> - Schema defined in BigQuery, data stays in external source
>
> **Benefits:**
> - No ingestion cost — no need to load data
> - Query data directly from GCS (CSV, Parquet, JSON files)
> - Good for: one-time analysis, data exploration before deciding to load
>
> **Limitations:**
> - Slower than native tables — external source read every time
> - Cannot use partitioning or clustering
> - No caching
>
> ```sql
> -- Create external table pointing to GCS
> CREATE EXTERNAL TABLE project.dataset.external_orders
> OPTIONS (
>     format = 'CSV',
>     uris = ['gs://my-bucket/orders/*.csv']
> );
>
> -- Query it like any normal table
> SELECT region, SUM(amount) FROM project.dataset.external_orders
> GROUP BY region;
> ```

---

<a id="q24"></a>
### 🔷 Q24. What is BigQuery Data Transfer Service?

> **Definition:**
> - A fully managed service to automatically transfer data into BigQuery on a scheduled basis
> - Set up once — runs automatically without manual intervention
>
> **Supported data sources:**
> - Google services: Google Analytics, Google Ads, YouTube, Google Play
> - Cloud storage: Amazon S3, Azure Blob Storage
> - Data warehouses: Teradata, Amazon Redshift
> - SaaS applications: Salesforce, Looker
>
> **Benefits:**
> - Automated — no manual data pipeline needed
> - Scheduled — daily, hourly or custom schedule
> - Managed by Google — no infrastructure to maintain
>
> **Use case for Data Engineers:**
> - Automatically pull Google Analytics data into BigQuery daily
> - Replicate Salesforce data into BigQuery for BI reporting
> - Migrate historical data from Amazon Redshift to BigQuery

---

<a id="q25"></a>
### 🔷 Q25. How does BigQuery handle Security?

> **1. Encryption:**
> - Data encrypted at rest by default using Google's AES-256 encryption
> - Data encrypted in transit using HTTPS/TLS
> - Customer-managed encryption keys (CMEK) available for extra control
>
> **2. Identity and Access Management (IAM):**
> - Control who can access datasets, tables and views
> - Assign roles at project, dataset or table level
> - Row-level and column-level security available
>
> **3. Column-level Security:**
> - Apply access policies on specific columns (e.g., hide PII data)
> - Policy tags restrict which users can see sensitive columns
>
> **4. Row-level Security:**
> - Control which rows of a table each user can see
> - Based on user identity or group membership
>
> **5. Audit Logging:**
> - All BigQuery activity logged in Cloud Audit Logs
> - Track who ran what query, when, on which table
>
> **6. Compliance:**
> - GDPR, HIPAA, PCI DSS, SOC 2 compliant
> - Data residency controls — specify region where data is stored

---

<a id="q26"></a>
### 🔷 Q26. What are IAM Roles in BigQuery?

| Role | Permissions |
|---|---|
| BigQuery Admin | Full control — create, delete, manage everything |
| BigQuery Data Owner | Read, write, delete tables and datasets |
| BigQuery Data Editor | Read and write data, cannot delete datasets |
| BigQuery Data Viewer | Read-only access to data |
| BigQuery Job User | Run queries but cannot see tables |
| BigQuery User | Run queries and see dataset metadata |

> **Best practice:**
> - Give minimum permissions needed (principle of least privilege)
> - Use Data Viewer for analysts — they only need to read data
> - Use Data Editor for data pipelines — they need to write data
> - Use Admin only for DBAs and infrastructure teams

---

<a id="q27"></a>
### 🔷 Q27. What is a BigQuery Job?

> **Definition:**
> - A job is an action BigQuery runs on your behalf — asynchronously in the background
>
> **Types of BigQuery Jobs:**
> - **Query Job** — runs a SQL query
> - **Load Job** — loads data from GCS or local file into a table
> - **Export Job** — exports table data to GCS
> - **Copy Job** — copies one table to another
>
> **Job properties:**
> - Each job has a unique Job ID
> - Jobs can be monitored in the BigQuery console under Job History
> - Jobs have statuses: PENDING, RUNNING, DONE, ERROR
>
> **Useful for Data Engineers:**
> - Track long-running pipelines via Job IDs
> - Monitor job performance and slot usage
> - Set up alerts for job failures

---

<a id="q28"></a>
### 🔷 Q28. What is a Dry Run in BigQuery?

> **Definition:**
> - A Dry Run is a way to estimate the cost of a query WITHOUT actually running it
> - BigQuery validates the query and returns the estimated bytes that would be scanned
> - No charges — Dry Run is free
>
> **How to use:**
> ```python
> from google.cloud import bigquery
>
> client = bigquery.Client()
>
> job_config = bigquery.QueryJobConfig(dry_run=True)
>
> query = "SELECT * FROM project.dataset.orders WHERE order_date = '2024-01-15'"
>
> job = client.query(query, job_config=job_config)
>
> print(f"Estimated bytes to scan: {job.total_bytes_processed}")
> print(f"Estimated cost: ${job.total_bytes_processed / 1e12 * 5:.4f}")
> ```
>
> **Best practice:**
> - Always dry run expensive queries before executing
> - In BigQuery console — write query → green checkmark shows estimated bytes
> - Red exclamation mark means query has syntax errors

---

<a id="q29"></a>
### 🔷 Q29. What is Query Caching in BigQuery?

> **Definition:**
> - BigQuery automatically caches query results for 24 hours
> - If you run the exact same query again within 24 hours, BigQuery returns cached results instantly
> - Cached queries are completely FREE — no bytes charged
>
> **Conditions for cache to work:**
> - Same query text (even spaces matter)
> - Same table data (no changes since last run)
> - Same user credentials
> - Query does not use non-deterministic functions like `NOW()`, `CURRENT_TIMESTAMP()`
> - Query does not use streaming buffer tables
>
> **When cache is NOT used:**
> - If underlying table data changed
> - If query contains `NOW()` or `RAND()` — result changes each time
> - If you explicitly disable cache: `job_config.use_query_cache = False`

---

## 🔴 SECTION 4: BIGQUERY WITH PYTHON & PRACTICAL (Q30–Q40)

---

<a id="q30"></a>
### 🔷 Q30. What are the Advantages and Disadvantages of BigQuery?

> **✅ Advantages:**
> - Serverless — zero infrastructure management
> - Automatically scales to petabytes of data
> - Very fast — scans terabytes in seconds using Dremel
> - Standard SQL — easy to use for SQL users
> - Pay-as-you-go — cost-effective for variable workloads
> - Built-in ML (BigQuery ML) — no data movement needed
> - Integrates seamlessly with all Google Cloud services
> - Real-time streaming ingestion support
> - Strong security — encryption, IAM, audit logs, compliance
> - Data sharing across teams without copying data
>
> **❌ Disadvantages:**
> - Not good for OLTP — frequent INSERT/UPDATE/DELETE is expensive and slow
> - LIMIT does not reduce cost — can lead to unexpected bills
> - Locked into GCP ecosystem — hard to migrate away
> - Limited customization — fully managed means less control
> - Complex queries and nested data (ARRAY, STRUCT) have learning curve
> - No traditional row-level indexes — point lookups are slow
> - Expensive for high volume of small frequent queries
> - Cross-region table joins not directly supported

---

<a id="q31"></a>
### 🔷 Q31. What is the difference between BigQuery and Cloud SQL?

| | BigQuery | Cloud SQL |
|---|---|---|
| Type | Data warehouse (OLAP) | Relational database (OLTP) |
| Best for | Analytics, large-scale reporting | Transactional apps, web apps |
| Architecture | Serverless, columnar | Server-based, row storage |
| Scale | Petabytes automatically | Terabytes, manual scaling |
| Transactions | Limited ACID support | Full ACID transactions |
| Query speed on big data | Very fast | Slower |
| Cost | Per bytes scanned | Per instance/hour |
| Use case | Business intelligence, data pipelines | Application backend database |

---

<a id="q32"></a>
### 🔷 Q32. What is BigQuery INFORMATION_SCHEMA?

> **Definition:**
> - INFORMATION_SCHEMA is a set of read-only views that provide metadata about BigQuery resources
> - Use it to monitor jobs, tables, queries and costs
>
> ```sql
> -- See all tables in a dataset
> SELECT table_name, row_count, size_bytes
> FROM project.dataset.INFORMATION_SCHEMA.TABLES;
>
> -- See recent query jobs and bytes processed
> SELECT job_id, query, total_bytes_processed, total_slot_ms
> FROM region-us.INFORMATION_SCHEMA.JOBS_BY_PROJECT
> WHERE creation_time > TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 24 HOUR)
> ORDER BY total_bytes_processed DESC
> LIMIT 10;
>
> -- See table partition info
> SELECT partition_id, total_rows, total_logical_bytes
> FROM project.dataset.INFORMATION_SCHEMA.PARTITIONS
> WHERE table_name = 'orders';
> ```
>
> **Data Engineering uses:**
> - Monitor which queries are most expensive
> - Track table sizes and partition sizes
> - Audit who ran which queries
> - Identify tables that need optimization

---

<a id="q33"></a>
### 🔷 Q33. How do you connect Python to BigQuery?

> **Install BigQuery Python client:**
> ```bash
> pip install google-cloud-bigquery pandas
> ```
>
> **Connect and run queries:**
> ```python
> from google.cloud import bigquery
> import pandas as pd
>
> # Initialize client (uses Application Default Credentials)
> client = bigquery.Client(project="my-gcp-project")
>
> # Run a query and load into Pandas DataFrame
> query = """
>     SELECT region, SUM(amount) as total_sales
>     FROM `project.dataset.orders`
>     WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31'
>     GROUP BY region
>     ORDER BY total_sales DESC
> """
>
> df = client.query(query).to_dataframe()
> print(df.head())
>
> # Write DataFrame back to BigQuery
> df.to_gbq(
>     destination_table="project.dataset.results",
>     project_id="my-gcp-project",
>     if_exists="replace"  # or 'append'
> )
> ```
>
> **Load data from GCS to BigQuery:**
> ```python
> job_config = bigquery.LoadJobConfig(
>     source_format=bigquery.SourceFormat.CSV,
>     skip_leading_rows=1,
>     autodetect=True,
> )
>
> uri = "gs://my-bucket/orders.csv"
> table_id = "project.dataset.orders"
>
> load_job = client.load_table_from_uri(uri, table_id, job_config=job_config)
> load_job.result()  # wait for job to complete
> print("Data loaded successfully")
> ```

---

<a id="q34"></a>
### 🔷 Q34. How do you write a BigQuery SQL query with Partitioned tables?

> ```sql
> -- Always filter on partition column to reduce cost
> SELECT
>     region,
>     product_category,
>     SUM(amount) as total_sales,
>     COUNT(*) as order_count,
>     AVG(amount) as avg_order_value
> FROM `project.dataset.orders`
> WHERE order_date BETWEEN '2024-01-01' AND '2024-03-31'  -- partition filter
>   AND region = 'East'                                     -- cluster filter
> GROUP BY region, product_category
> ORDER BY total_sales DESC;
>
> -- Month-over-Month comparison using partitioned table
> WITH monthly AS (
>     SELECT
>         DATE_TRUNC(order_date, MONTH) as month,
>         SUM(amount) as total_sales
>     FROM `project.dataset.orders`
>     WHERE order_date >= '2023-01-01'  -- partition filter
>     GROUP BY 1
> )
> SELECT
>     month,
>     total_sales,
>     LAG(total_sales) OVER (ORDER BY month) as prev_month,
>     ROUND((total_sales - LAG(total_sales) OVER (ORDER BY month)) * 100.0
>           / LAG(total_sales) OVER (ORDER BY month), 2) as mom_growth_pct
> FROM monthly
> ORDER BY month;
> ```

---

<a id="q35"></a>
### 🔷 Q35. What is ARRAY and STRUCT in BigQuery?

> **ARRAY:**
> - A column that contains multiple values of the same type in one cell
> - Like a list inside a single column
>
> ```sql
> -- Table with ARRAY column
> SELECT name, scores
> FROM students;
> -- Result: Raj | [85, 90, 78, 92]
>
> -- Access array elements
> SELECT name, scores[OFFSET(0)] as first_score FROM students;
>
> -- Check if value exists in array
> SELECT name FROM students WHERE 90 IN UNNEST(scores);
> ```
>
> **STRUCT:**
> - A column that contains multiple fields of different types — like a nested record
> - Similar to a JSON object inside a column
>
> ```sql
> -- Table with STRUCT column
> SELECT name, address.city, address.pincode
> FROM customers;
> -- address is a STRUCT with city and pincode fields
>
> -- Create a STRUCT
> SELECT STRUCT('Mumbai' AS city, '400001' AS pincode) AS address;
> ```

---

<a id="q36"></a>
### 🔷 Q36. What is UNNEST in BigQuery?

> **Definition:**
> - UNNEST flattens an ARRAY into multiple rows
> - Used to work with repeated/nested data in BigQuery
>
> ```sql
> -- Original data: one row per student with array of scores
> -- After UNNEST: one row per score
>
> SELECT name, score
> FROM students,
> UNNEST(scores) AS score;
>
> -- Result:
> -- Raj | 85
> -- Raj | 90
> -- Raj | 78
>
> -- Aggregate after UNNEST
> SELECT name, AVG(score) as avg_score
> FROM students,
> UNNEST(scores) AS score
> GROUP BY name;
> ```

---

<a id="q37"></a>
### 🔷 Q37. How do you handle duplicate data in BigQuery?

> **Method 1 — Using ROW_NUMBER:**
> ```sql
> -- Remove duplicates — keep first occurrence
> CREATE OR REPLACE TABLE project.dataset.orders AS
> SELECT * EXCEPT(row_num)
> FROM (
>     SELECT *,
>            ROW_NUMBER() OVER (
>                PARTITION BY order_id
>                ORDER BY created_at ASC
>            ) as row_num
>     FROM project.dataset.orders
> )
> WHERE row_num = 1;
> ```
>
> **Method 2 — MERGE for UPSERT:**
> ```sql
> -- Insert new, update existing — no duplicates
> MERGE project.dataset.orders AS target
> USING staging_orders AS source
> ON target.order_id = source.order_id
> WHEN MATCHED THEN
>     UPDATE SET amount = source.amount, updated_at = CURRENT_TIMESTAMP()
> WHEN NOT MATCHED THEN
>     INSERT (order_id, amount, region, created_at)
>     VALUES (source.order_id, source.amount, source.region, CURRENT_TIMESTAMP());
> ```

---

<a id="q38"></a>
### 🔷 Q38. What is the Table Snapshot and Table Clone in BigQuery?

> **Table Snapshot:**
> - A read-only point-in-time copy of a table
> - Preserves the table state at a specific moment
> - Useful for: backup before data transformation, audit, disaster recovery
> - Storage efficient — only stores changes from base table
>
> **Table Clone:**
> - A writable copy of a table
> - Can be modified independently without affecting the original
> - Storage efficient — only stores delta (differences) from base table
> - Useful for: testing transformations without affecting production data
>
> ```sql
> -- Create snapshot
> CREATE SNAPSHOT TABLE project.dataset.orders_backup
> CLONE project.dataset.orders
> OPTIONS (expiration_timestamp = TIMESTAMP_ADD(CURRENT_TIMESTAMP(), INTERVAL 7 DAY));
>
> -- Create clone
> CREATE TABLE project.dataset.orders_test
> CLONE project.dataset.orders;
> ```

---

<a id="q39"></a>
### 🔷 Q39. What is BigQuery Omni?

> **Definition:**
> - BigQuery Omni is a multi-cloud analytics solution from Google
> - Allows you to run BigQuery SQL queries on data stored in other clouds — AWS S3 and Azure Blob Storage
> - Without moving or copying data out of those clouds
>
> **Benefits:**
> - Analyze data across GCP, AWS and Azure in one place
> - No data movement — reduces cost and complexity
> - Single SQL interface for multi-cloud data
>
> **Use case:**
> - Company has data in both AWS and GCP
> - Use BigQuery Omni to join and analyze without migrating all data to GCP

---

<a id="q40"></a>
### 🔷 Q40. How do you monitor and debug BigQuery jobs?

> **1. BigQuery Console — Job History:**
> - View all past queries, execution time, bytes processed
> - See query execution plan (stages, slots, time per stage)
> - Identify slow stages and bottlenecks
>
> **2. INFORMATION_SCHEMA.JOBS:**
> ```sql
> -- Find top 10 most expensive queries today
> SELECT
>     user_email,
>     query,
>     ROUND(total_bytes_processed / 1e9, 2) as gb_scanned,
>     ROUND(total_bytes_processed / 1e12 * 5, 4) as est_cost_usd,
>     total_slot_ms,
>     creation_time
> FROM region-us.INFORMATION_SCHEMA.JOBS_BY_PROJECT
> WHERE DATE(creation_time) = CURRENT_DATE()
>   AND state = 'DONE'
>   AND job_type = 'QUERY'
> ORDER BY total_bytes_processed DESC
> LIMIT 10;
> ```
>
> **3. Execution Plan:**
> - Click "Execution Details" tab after running a query
> - Shows each stage: read, compute, write
> - Shows input/output rows and bytes for each stage
> - Identify data skew — one stage takes 90% of time
>
> **4. Stackdriver / Cloud Monitoring:**
> - Set up alerts for job failures
> - Monitor slot utilization over time
> - Track query counts and costs per day

---

## 🟣 SECTION 5: EXTRA IMPORTANT TOPICS (Q41–Q55)

---

<a id="q41"></a>
### 🔷 Q41. BigQuery vs Snowflake vs Redshift — Key Differences (Very Important)

| Feature | BigQuery | Snowflake | Amazon Redshift |
|---|---|---|---|
| Cloud | GCP only (+ Omni for multi-cloud) | Multi-cloud (AWS, Azure, GCP) | AWS only |
| Architecture | Serverless, fully managed | Separate compute + storage | Cluster-based (shared-nothing) |
| Pricing | Per bytes scanned + storage | Per second compute + storage | Per node/hour or serverless |
| Scaling | Fully auto, instant | Auto via virtual warehouses | Manual or auto cluster scaling |
| Setup | Zero setup — log in and query | Minimal setup | Needs cluster config |
| ML Built-in | ✅ BigQuery ML (SQL-based) | ✅ Snowpark ML | ✅ SageMaker integration |
| Best for | GCP ecosystem, serverless analytics | Multi-cloud, data sharing | AWS ecosystem, structured data |
| Cost pattern | Unpredictable (based on bytes) | Predictable (per second) | Predictable (per node) |
| Semi-structured | ARRAY, STRUCT, JSON | VARIANT (JSON, Avro, Parquet) | SUPER type, Spectrum |

> **When to choose what:**
> - **BigQuery** — already on GCP, want serverless, need real-time analytics
> - **Snowflake** — multi-cloud or cross-company data sharing needed
> - **Redshift** — already on AWS, tight AWS ecosystem integration needed

---

<a id="q42"></a>
### 🔷 Q42. What is a Data Lake vs Data Warehouse vs Data Lakehouse?

| | Data Lake | Data Warehouse | Data Lakehouse |
|---|---|---|---|
| Data type | Raw, unstructured + structured | Structured, cleaned | Both raw and structured |
| Schema | Schema-on-read | Schema-on-write | Both supported |
| Storage cost | Very cheap (object storage) | More expensive | Cheap (object storage) |
| Query speed | Slower | Faster | Fast |
| Tools | GCS, S3, HDFS | BigQuery, Redshift, Snowflake | Delta Lake, BigLake |
| Use case | Store everything, explore later | BI reporting, analytics | Modern unified analytics |

> **BigQuery as Data Lakehouse:**
> - BigQuery supports querying data directly from GCS (external tables)
> - BigLake extends BigQuery to query across GCS and BigQuery together
> - This makes BigQuery act as a Data Lakehouse — best of both worlds

---

<a id="q43"></a>
### 🔷 Q43. What is Google Cloud Pub/Sub and how does it work with BigQuery?

> **What is Pub/Sub:**
> - Pub/Sub is Google Cloud's fully managed real-time messaging service
> - Works like Kafka — producers publish messages, consumers subscribe and receive them
> - Decouples data producers from consumers
> - Used for real-time event streaming, log ingestion, IoT data
>
> **Pub/Sub → BigQuery pipeline:**
> - Events published to Pub/Sub topic (user clicks, orders, sensor data)
> - BigQuery Subscription reads from Pub/Sub and writes directly to BigQuery table
> - No Dataflow needed for simple streaming pipelines
>
> ```
> App/IoT → Pub/Sub Topic → BigQuery Subscription → BigQuery Table → Dashboard
> ```
>
> **Python example:**
> ```python
> from google.cloud import pubsub_v1
> import json
>
> publisher = pubsub_v1.PublisherClient()
> topic_path = "projects/my-project/topics/orders"
>
> # Publish a message
> data = json.dumps({"order_id": 101, "amount": 5000, "region": "East"})
> publisher.publish(topic_path, data.encode("utf-8"))
> ```
>
> **Key points:**
> - Pub/Sub = Google's equivalent of Apache Kafka
> - At-least-once delivery — messages may arrive more than once
> - Use deduplication logic in BigQuery when consuming from Pub/Sub

---

<a id="q44"></a>
### 🔷 Q44. What is Google Cloud Dataflow?

> **Definition:**
> - Dataflow is Google Cloud's fully managed stream and batch data processing service
> - Based on Apache Beam — write pipeline once, run on both batch and streaming
> - Serverless — auto-scales workers based on data volume
>
> **Common Dataflow pipelines for Data Engineers:**
> - Read from Pub/Sub → Transform → Write to BigQuery (real-time)
> - Read from GCS (CSV/Parquet) → Clean/Transform → Write to BigQuery (batch)
> - Read from BigQuery → Process → Write to GCS (export pipeline)
>
> ```python
> import apache_beam as beam
> from apache_beam.options.pipeline_options import PipelineOptions
>
> options = PipelineOptions(runner='DataflowRunner',
>                           project='my-project',
>                           region='us-central1')
>
> with beam.Pipeline(options=options) as p:
>     (p
>      | 'Read CSV' >> beam.io.ReadFromText('gs://bucket/data.csv')
>      | 'Parse' >> beam.Map(lambda line: line.split(','))
>      | 'Write to BQ' >> beam.io.WriteToBigQuery('project:dataset.table')
>     )
> ```
>
> **Dataflow vs Kafka:**
> - Dataflow = managed processing (transform data)
> - Pub/Sub/Kafka = managed messaging (transport data)
> - Together: Pub/Sub (ingest) → Dataflow (process) → BigQuery (store)

---

<a id="q45"></a>
### 🔷 Q45. What are Common BigQuery SQL Functions for Data Engineers?

> **Date/Time Functions:**
> ```sql
> CURRENT_DATE()                          -- today's date
> CURRENT_TIMESTAMP()                     -- current timestamp
> DATE_TRUNC(date, MONTH)                 -- first day of month: 2024-01-01
> DATE_TRUNC(date, YEAR)                  -- first day of year: 2024-01-01
> EXTRACT(YEAR FROM date)                 -- get year: 2024
> DATE_DIFF(date1, date2, DAY)            -- difference in days
> DATE_ADD(date, INTERVAL 7 DAY)          -- add 7 days
> TIMESTAMP_TRUNC(ts, HOUR)               -- truncate to hour
> FORMAT_DATE('%Y-%m', date)              -- format: '2024-01'
> ```
>
> **String Functions:**
> ```sql
> UPPER(str), LOWER(str)                  -- case conversion
> TRIM(str), LTRIM(str), RTRIM(str)       -- remove whitespace
> SUBSTR(str, 1, 5)                       -- substring
> CONCAT(str1, str2)                      -- join strings
> SPLIT(str, ',')                         -- split to array
> REGEXP_CONTAINS(str, r'\d+')            -- regex match check
> REGEXP_REPLACE(str, r'\s+', ' ')        -- regex replace
> ```
>
> **Conditional Functions:**
> ```sql
> COALESCE(col1, col2, 'default')         -- first non-null
> IF(condition, true_val, false_val)       -- simple if
> IFNULL(col, 0)                          -- replace null
> NULLIF(val, 0)                          -- return null if equal
> CASE WHEN ... THEN ... ELSE ... END     -- complex conditions
> ```
>
> **Aggregate Functions:**
> ```sql
> COUNT(*), COUNT(DISTINCT col)
> SUM(col), AVG(col), MIN(col), MAX(col)
> ARRAY_AGG(col)                          -- aggregate into array
> STRING_AGG(col, ',')                    -- join strings with separator
> APPROX_COUNT_DISTINCT(col)              -- fast approximate distinct count
> ```

---

<a id="q46"></a>
### 🔷 Q46. What are Window Functions in BigQuery?

> ```sql
> -- RANK and DENSE_RANK
> SELECT name, region, sales,
>     RANK() OVER (PARTITION BY region ORDER BY sales DESC) as rank,
>     DENSE_RANK() OVER (PARTITION BY region ORDER BY sales DESC) as dense_rank
> FROM orders;
>
> -- ROW_NUMBER — unique row number
> SELECT *, ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date) as rn
> FROM orders;
>
> -- Running total and moving average
> SELECT order_date, amount,
>     SUM(amount) OVER (ORDER BY order_date) as running_total,
>     AVG(amount) OVER (ORDER BY order_date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) as weekly_avg
> FROM daily_sales;
>
> -- LAG and LEAD — previous and next values
> SELECT order_date, sales,
>     LAG(sales, 1) OVER (ORDER BY order_date) as prev_day_sales,
>     LEAD(sales, 1) OVER (ORDER BY order_date) as next_day_sales,
>     sales - LAG(sales, 1) OVER (ORDER BY order_date) as daily_change
> FROM daily_sales;
>
> -- NTILE — divide into N equal groups (quartiles)
> SELECT name, sales,
>     NTILE(4) OVER (ORDER BY sales) as quartile
> FROM orders;
> -- quartile 1 = bottom 25%, quartile 4 = top 25%
> ```

---

<a id="q47"></a>
### 🔷 Q47. What is Table Expiration and Partition Expiration in BigQuery?

> **Table Expiration:**
> - Automatically deletes a table after a specified time
> - Useful for temporary or staging tables
>
> ```sql
> -- Create table that expires in 7 days
> CREATE TABLE project.dataset.temp_results
> OPTIONS (expiration_timestamp = TIMESTAMP_ADD(CURRENT_TIMESTAMP(), INTERVAL 7 DAY))
> AS SELECT * FROM orders WHERE order_date = CURRENT_DATE();
> ```
>
> **Partition Expiration:**
> - Automatically deletes old partitions after specified days
> - Useful for managing storage costs on time-series tables
> - Example: keep only last 365 days of data
>
> ```sql
> -- Create partitioned table with 365 day partition expiry
> CREATE TABLE project.dataset.events
> PARTITION BY DATE(event_timestamp)
> OPTIONS (partition_expiration_days = 365)
> AS SELECT * FROM raw_events;
> -- Partitions older than 365 days auto-deleted
> ```
>
> **Why important for Data Engineers:**
> - Automatic storage management — no manual cleanup needed
> - Reduces storage costs for large time-series tables
> - Ensures compliance — old data automatically removed

---

<a id="q48"></a>
### 🔷 Q48. What is BigQuery Scheduled Queries?

> **Definition:**
> - BigQuery allows you to schedule SQL queries to run automatically at specific times
> - No need for external schedulers like Airflow for simple use cases
>
> **How to set up:**
> - In BigQuery console: write query → click "Schedule" → set frequency
> - Can overwrite a table, append to a table, or create new table each run
>
> **Use cases for Data Engineers:**
> - Daily aggregation — run sales summary every night at midnight
> - Data refresh — refresh materialized data every hour
> - Report generation — auto-generate weekly reports
>
> ```sql
> -- Example scheduled query — runs daily at 2 AM
> -- Creates daily sales summary
> CREATE OR REPLACE TABLE project.dataset.daily_sales_summary AS
> SELECT
>     DATE(order_date) as date,
>     region,
>     SUM(amount) as total_sales,
>     COUNT(*) as order_count
> FROM project.dataset.orders
> WHERE DATE(order_date) = DATE_SUB(CURRENT_DATE(), INTERVAL 1 DAY)
> GROUP BY 1, 2;
> ```

---

<a id="q49"></a>
### 🔷 Q49. How do you implement SCD Type 2 in BigQuery?

> **SCD Type 2** — keep full history by adding new rows with version tracking
>
> ```sql
> -- SCD Type 2 table structure in BigQuery
> CREATE TABLE project.dataset.customers_scd2 (
>     surrogate_key INT64,        -- unique key for each version
>     customer_id INT64,          -- natural/business key
>     name STRING,
>     city STRING,
>     start_date DATE,
>     end_date DATE,
>     is_current BOOL
> )
> PARTITION BY start_date;
>
> -- Step 1: Mark old record as inactive
> UPDATE project.dataset.customers_scd2
> SET end_date = DATE_SUB(CURRENT_DATE(), INTERVAL 1 DAY),
>     is_current = FALSE
> WHERE customer_id = 101 AND is_current = TRUE;
>
> -- Step 2: Insert new version
> INSERT INTO project.dataset.customers_scd2
> VALUES (new_surrogate_key, 101, 'Raj Kumar', 'Mumbai',
>         CURRENT_DATE(), DATE('9999-12-31'), TRUE);
>
> -- Query current records only
> SELECT * FROM project.dataset.customers_scd2
> WHERE is_current = TRUE;
>
> -- Query historical records for a customer
> SELECT * FROM project.dataset.customers_scd2
> WHERE customer_id = 101
> ORDER BY start_date;
> ```

---

<a id="q50"></a>
### 🔷 Q50. What are BigQuery Best Practices for Data Engineers?

> **Query Cost Optimization:**
> - Always select specific columns — never `SELECT *`
> - Always filter on partition columns in WHERE clause
> - Use Dry Run to estimate cost before running
> - Use `LIMIT` with `ORDER BY` only — not to reduce cost
> - Avoid using functions on partition columns (breaks pruning)
> - Use `APPROX_COUNT_DISTINCT` instead of `COUNT(DISTINCT)` for large datasets
>
> **Performance Optimization:**
> - Partition large tables by date
> - Cluster by columns used in WHERE and GROUP BY
> - Use Materialized Views for frequently-run aggregations
> - Filter early — reduce data before JOINs
> - Put the larger table first in JOIN (left side)
> - Avoid self-joins on large tables — use window functions instead
>
> **Pipeline Best Practices:**
> - Use Parquet format for batch loads — fastest and cheapest
> - Use streaming only when real-time truly needed — batch is free
> - Always handle duplicates — use MERGE or ROW_NUMBER deduplication
> - Set partition expiration for staging and temporary tables
> - Use table snapshots before major data transformations
>
> **Cost Management:**
> - Set budget alerts in GCP Billing
> - Use INFORMATION_SCHEMA.JOBS to find expensive queries
> - Reserve slots for predictable heavy workloads
> - Use long-term storage discount — tables not modified in 90 days cost half

---

<a id="q51"></a>
### 🔷 Q51. What is the difference between Standard SQL and Legacy SQL in BigQuery?

| | Standard SQL | Legacy SQL |
|---|---|---|
| Status | Current — preferred | Old — deprecated |
| Table reference | `` `project.dataset.table` `` | `[project:dataset.table]` |
| NULL handling | Standard SQL rules | Different behavior |
| Window functions | ✅ Full support | Limited support |
| WITH / CTE | ✅ Supported | ❌ Not supported |
| DML (INSERT/UPDATE) | ✅ Supported | ❌ Not supported |
| Subqueries | ✅ Full support | Limited |

> **Always use Standard SQL** — Legacy SQL is deprecated and has fewer features.
> In BigQuery console: Standard SQL is selected by default.

---

<a id="q52"></a>
### 🔷 Q52. What is BigQuery's MERGE statement?

> **MERGE = UPSERT** — Insert new rows and update existing rows in one statement
> Very important for data pipelines to avoid duplicates

> ```sql
> -- MERGE target table with staging data
> MERGE project.dataset.orders AS target
> USING project.dataset.staging_orders AS source
> ON target.order_id = source.order_id
>
> -- Update if record already exists
> WHEN MATCHED THEN
>     UPDATE SET
>         amount = source.amount,
>         status = source.status,
>         updated_at = CURRENT_TIMESTAMP()
>
> -- Insert if record is new
> WHEN NOT MATCHED THEN
>     INSERT (order_id, customer_id, amount, region, status, created_at)
>     VALUES (source.order_id, source.customer_id, source.amount,
>             source.region, source.status, CURRENT_TIMESTAMP())
>
> -- Delete if source marks it as deleted
> WHEN NOT MATCHED BY SOURCE AND target.status != 'DELETED' THEN
>     UPDATE SET status = 'DELETED';
> ```
>
> **Pipeline flow using MERGE:**
> - Load new data into staging table
> - Run MERGE — auto handles inserts and updates
> - No duplicate rows ever

---

<a id="q53"></a>
### 🔷 Q53. How do you load data from GCS to BigQuery using Python?

> ```python
> from google.cloud import bigquery
>
> client = bigquery.Client(project="my-project")
>
> # Define load job config
> job_config = bigquery.LoadJobConfig(
>     source_format=bigquery.SourceFormat.PARQUET,  # or CSV, JSON, AVRO
>     write_disposition=bigquery.WriteDisposition.WRITE_APPEND,  # or WRITE_TRUNCATE
>     autodetect=True,                              # auto-detect schema
> )
>
> # GCS file path
> uri = "gs://my-bucket/data/orders/2024-01-15/*.parquet"
> table_id = "my-project.my_dataset.orders"
>
> # Start load job
> load_job = client.load_table_from_uri(uri, table_id, job_config=job_config)
>
> # Wait for job to complete
> load_job.result()
>
> # Check result
> table = client.get_table(table_id)
> print(f"Loaded {table.num_rows} rows into {table_id}")
> ```
>
> **Write dispositions:**
> - `WRITE_TRUNCATE` — replace all existing data (full load)
> - `WRITE_APPEND` — add to existing data (incremental load)
> - `WRITE_EMPTY` — only write if table is empty

---

<a id="q54"></a>
### 🔷 Q54. What is BigQuery's Time Travel feature?

> **Definition:**
> - BigQuery keeps a history of all changes to a table for up to 7 days
> - You can query data as it was at any point in the last 7 days
> - Useful for: recovering accidentally deleted data, auditing changes, debugging pipelines
>
> ```sql
> -- Query table as it was 24 hours ago
> SELECT * FROM project.dataset.orders
> FOR SYSTEM_TIME AS OF TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 24 HOUR);
>
> -- Query table as it was at a specific time
> SELECT * FROM project.dataset.orders
> FOR SYSTEM_TIME AS OF '2024-01-15 10:00:00 UTC';
>
> -- Recover accidentally deleted data
> CREATE OR REPLACE TABLE project.dataset.orders AS
> SELECT * FROM project.dataset.orders
> FOR SYSTEM_TIME AS OF TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 1 HOUR);
> ```
>
> **Key points:**
> - Available for 7 days by default
> - No extra cost for time travel storage (included)
> - Very useful before running destructive operations (DELETE, UPDATE, TRUNCATE)

---

<a id="q55"></a>
### 🔷 Q55. What is a Real-World Data Pipeline using BigQuery?

> **Complete end-to-end pipeline example:**
>
> ```
> Data Sources → Ingestion → Processing → Storage → Serving
> ```
>
> **Step by step:**
> - **Ingestion:**
>   - REST API → Pub/Sub → BigQuery Streaming (real-time events)
>   - Files on GCS → Batch Load Job → BigQuery (historical data)
>   - Database → Cloud Dataflow → BigQuery (CDC pipeline)
>
> - **Storage in BigQuery:**
>   - Raw layer: `raw_dataset.orders` (partitioned by date, no transformation)
>   - Staging layer: `staging_dataset.orders_cleaned` (cleaned, deduplicated)
>   - Reporting layer: `reporting_dataset.daily_sales` (aggregated, ready for BI)
>
> - **Processing:**
>   - BigQuery Scheduled Queries for daily aggregations
>   - Dataflow for complex transformations
>   - Python scripts with BigQuery client for custom logic
>
> - **Serving:**
>   - Looker / Data Studio dashboard reads from reporting layer
>   - API reads from BigQuery via Python client
>   - Exports to GCS for downstream systems
>
> **This is what JSW or any large company's pipeline looks like — mention this in interview!**

---

## 💡 FINAL QUICK REVISION

**Architecture:**
> ✅ Dremel = query engine | Colossus = storage | Jupiter = network | Borg = cluster manager
> ✅ Storage and Compute SEPARATED — scale independently, pay only when querying
> ✅ Columnar storage = reads only columns needed — faster + cheaper
> ✅ Slots = units of compute | On-demand = pay per bytes | Flat-rate = reserved slots

**Partitioning & Clustering:**
> ✅ Partitioning = divides table into segments — skips entire partitions (partition pruning)
> ✅ Clustering = organizes rows by column values — skips data blocks (block pruning)
> ✅ Use BOTH together — partition by date, cluster by region/category
> ✅ LIMIT does NOT reduce cost — BigQuery charges for bytes scanned

**Cost Optimization:**
> ✅ SELECT specific columns — never SELECT *
> ✅ Filter on partition column — biggest cost saver
> ✅ Dry Run before running — check estimated cost for free
> ✅ Materialized Views for repeated aggregation queries
> ✅ Cache — repeated same queries are FREE for 24 hours
> ✅ Long-term storage discount — tables not modified in 90+ days cost half

**Loading & Ingestion:**
> ✅ Batch loading = FREE | Streaming ingestion = charged per GB
> ✅ Best file format = Parquet (columnar, compressed, fastest)
> ✅ External tables = query GCS/Sheets/Cloud SQL without loading
> ✅ Pub/Sub → BigQuery = real-time streaming pipeline

**Practical SQL:**
> ✅ MERGE = UPSERT — insert new + update existing, no duplicates
> ✅ Time Travel = query table as it was up to 7 days ago
> ✅ UNNEST = flatten ARRAY into rows
> ✅ DATE_TRUNC, EXTRACT, DATE_DIFF = most used date functions
> ✅ ROW_NUMBER() OVER (PARTITION BY...) = deduplicate records

**Comparisons:**
> ✅ BigQuery = GCP, serverless, per bytes scanned
> ✅ Snowflake = multi-cloud, per second compute
> ✅ Redshift = AWS, per node/hour
> ✅ BigQuery = Data Warehouse + can act as Data Lakehouse (via BigLake + GCS)

---

*All the best Rajkishor! Python + SQL + Tableau + BigQuery = JSW Data Engineer fully ready! 🎯*
