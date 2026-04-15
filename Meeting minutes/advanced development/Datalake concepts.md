| suConcept                  | Traditional approach                                           | Lakehouse‑style with ClickHouse MV                                                                    |
| -------------------------- | -------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Data source**            | Raw files in object store (S3, ADLS, GCS) or a staging table   | Raw “landing” table (e.g., `raw_events`) in ClickHouse                                                |
| **Transformation (ETL)**   | Batch jobs (Spark, Flink, dbt) scheduled by an orchestrator    | **Materialised view** definition that expresses the same transformation                               |
| **Scheduler/orchestrator** | Airflow DAG, Prefect flow, AWS Step Functions, etc.            | Implicit – ClickHouse updates the view automatically as data arrives                                  |
| **State handling**         | Job needs to keep track of processed offsets, watermarks, etc. | ClickHouse tracks internally which rows have been consumed (by using the underlying MergeTree engine) |
| **Latency**                | Usually minutes‑to‑hours (depends on schedule)                 | Near‑real‑time (as soon as a row is inserted into the source table)                                   |
|                            |                                                                |                                                                                                       |

**Upsert**
An upsert atomically inserts a new row if the key doesn’t exist or updates the existing row with that key when it does.

**ETL – Extract → Transform → Load**

At its core, ETL is a three‑stage pipeline that moves data from one (or many) source systems into a destination that is optimized for analytics, reporting, or downstream processing.

**Extract** - Connect to the source(s) and pull the raw data (files, relational tables, NoSQL stores, APIs, message queues, etc.). The goal is *high‑throughput, reliable* capture without altering the source. 
**Transform** - Clean, enrich, reshape, and aggregate the data so it fits the target schema and business rules. Transformations can be *stateless* (e.g., type‑casting) or *stateful* (e.g., rolling windows, joins). 
**Load** - Write the transformed data into the destination, often in a way that enables fast, concurrent reads. Loading strategies include *append‑only*, *upserts* (merge), or *re‑partitioned* writes.

**OLAP vs OLTP**
**OLAP vs OLTP – at a glance**

| Aspect | OLAP (Analytical) | OLTP (Transactional) |
|--------|-------------------|----------------------|
| **Primary Goal** | Fast, ad‑hoc analysis of large historical datasets | Efficient processing of many short, write‑heavy business transactions |
| **Typical Queries** | Complex aggregations, joins, window functions, scans across millions‑to‑billions of rows | Simple point‑lookups, inserts, updates, deletes on a single or few rows |
| **Schema Design** | Star / snowflake, denormalized, wide tables, many columns for pre‑computed dimensions | Normalized (3NF), narrow tables, many foreign keys, tight referential integrity |
| **Data Volume** | Tens of GB → many PB (historical / fact tables) | Typically MB → low‑hundred GB (current operational state) |
| **Concurrency Pattern** | Fewer concurrent users, long‑running queries (seconds‑to‑minutes) | Thousands‑of‑concurrent users, short‑lived queries (milliseconds‑to‑sub‑seconds) |
| **Latency Requirements** | Acceptable latency: seconds‑to‑minutes; emphasis on throughput | Sub‑second latency; emphasis on response time |
| **Consistency Model** | Often eventual or read‑committed; snapshots for reporting | Strong ACID (strict serializability or at least read‑committed) |
| **Indexes / Access Paths** | Bitmap, materialised aggregates, columnar compression, zone maps | B‑tree / hash indexes, primary key lookups |
| **Hardware Optimisation** | Large, sequential I/O, high‑throughput CPUs, columnar storage, distributed clusters | Low‑latency random I/O, high‑core‑count CPUs, in‑memory caches, row‑store databases |
| **Typical Systems** | ClickHouse, Snowflake, Redshift, BigQuery, Azure Synapse, Druid | PostgreSQL, MySQL, Oracle, SQL Server, MariaDB, CockroachDB, Aurora |
| **Backup / Retention** | Long‑term archiving, tiered storage (cold‑line) | Short‑term backups, point‑in‑time recovery |
| **Typical Use Cases** | Dashboards, BI, data mining, trend analysis, ML feature generation | Order entry, inventory updates, payments, user profile CRUD, session management |

**Clickhouse materialized view**
A **_materialised view_** is a table that is **automatically populated** by the result of a SELECT query that runs on another table (the _source_).