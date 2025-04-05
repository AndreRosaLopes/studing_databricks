# 1. Incremental Prossing with Spark Structured Streaming
What is streaming data? Continuously generated and unbounded data.

<img src="ADE/image-20.png" width="40%" />

<img src="ADE/image-21.png" width="40%" />

Use cases:
* Notifications
* Real-time reporting
* Incremental ETL
* Updata data to serve in real-time
* Real-time decision making (bank: credit card frauld...)
* Online ML

<img src="ADE/image-22.png" width="40%" />

<img src="ADE/image-23.png" width="40%" />

<img src="ADE/image-24.png" width="40%" />

<img src="ADE/image-25.png" width="40%" />

<img src="ADE/image-26.png" width="40%" />

<img src="ADE/image-27.png" width="40%" />

<img src="ADE/image-28.png" width="40%" />

## LAB-1.1. Reading from a Streaming Query

### Classes
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.ss/api/pyspark.sql.streaming.DataStreamReader.html" target="_blank">DataStreamReader</a>
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.ss/api/pyspark.sql.streaming.DataStreamWriter.html" target="_blank">DataStreamWriter</a>
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.ss/api/pyspark.sql.streaming.StreamingQuery.html" target="_blank">StreamingQuery</a>

**Obtain an initial streaming DataFrame from a Delta-format file source.**
```python
    df = (spark
        .readStream
        .format("delta")
        .load(DA.paths.events)
    )
```
**Apply some transformations, producing new streaming DataFrames.**
```python
from pyspark.sql.functions import col, approx_count_distinct, count
# import F. Also, cell imports in 
email_traffic_df = (df
                    .filter(col("traffic_source") == "email")
                    .withColumn("mobile", col("device").isin(["iOS", "Android"]))
                    .select("user_id", "event_timestamp", "mobile")
                   )
```

# 2. Structured Streaming
engine: Spark Structured Streaming

Spark Structured Streaming is a scalable, fault-tolerant stream processing framework built on Spark SQL engine. It uses existing structured APIs (DataFrames, SQL Engine) and provides similar API as batch processing API.

Features:

1. End-to-end
1. Exactly-once processing
1. Fault-tolerance

<img src="ADE/image-29.png" width="40%" />

* Micro-batch execution: acuumulate small batches of data and process each batch in **parallel**.
* Continuous execution (experimental): continuously listen for new data and process them **individually**. Overhead per record. Load balance issues.

<img src="ADE/image-30.png" width="40%" />

<img src="ADE/image-31.png" width="40%" />

<img src="ADE/image-32.png" width="40%" />

<img src="ADE/image-33.png" width="40%" />

<img src="ADE/image-34.png" width="40%" />

<img src="ADE/image-35.png" width="40%" />

<img src="ADE/image-36.png" width="40%" />

<img src="ADE/image-37.png" width="40%" />

<img src="ADE/image-38.png" width="40%" />

## Benefits
<img src="ADE/image-39.png" width="40%" />

<img src="ADE/image-40.png" width="40%" />

<img src="ADE/image-41.png" width="40%" />

<img src="ADE/image-42.png" width="40%" />

<img src="ADE/image-43.png" width="40%" />

<img src="ADE/image-44.png" width="40%" />

## Lab 1.1 Reading from a Streaming Query

##### Classes
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.ss/api/pyspark.sql.streaming.DataStreamReader.html" target="_blank">DataStreamReader</a>
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.ss/api/pyspark.sql.streaming.DataStreamWriter.html" target="_blank">DataStreamWriter</a>
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.ss/api/pyspark.sql.streaming.StreamingQuery.html" target="_blank">StreamingQuery</a>

## Aggregations, Time Windows, Watermarks

<img src="ADE/image-45.png" width="40%" />

<img src="ADE/image-46.png" width="40%" />

<img src="ADE/image-47.png" width="40%" />

Watermark - is a feature in Struct Streaming to allow you specify how late you expect to see data in event time.
Data later than threshold will be dropped ( not considered in aggregation)

Time Domain Skew

<img src="ADE/image-48.png" width="40%" />

<img src="ADE/image-49.png" width="40%" />

## Delta Live Tables

<img src="ADE/image-50.png" width="40%" />

<img src="ADE/image-51.png" width="40%" />

<img src="ADE/image-52.png" width="40%" />

With DLT, engineers are able to treat their data as code and apply modern software engineering best practices like testing, error handling, monitoring, and documentation to deploy reliable pipelines at scale.

<img src="ADE/image-53.png" width="40%" />

<img src="ADE/image-54.png" width="40%" />

<img src="ADE/image-55.png" width="40%" />

# Streaming ETL Patterns with DLT

<img src="ADE/image-56.png" width="40%" />

## Pattern #1

<img src="ADE/image-57.png" width="40%" />

TBLPROPERTIES(pipelines.reset.allowed=false) => If we delete raw data, the bronze table would not be recomputed.

The general best practice is to have a streaming table for bronze data and live tables for silver and gold, especially for pipelines that involve sensitive data.

## Pattern #2
<img src="ADE/image-58.png" width="40%" />


## Pattern #3
<img src="ADE/image-59.png" width="40%" />

<img src="ADE/image-60.png" width="40%" />

The problem of simplex:
* Parsing to multiple tables requires multiple streams, as well as a separate process to write each silver table.
* Skew in data volume and velocity can significantly impact throughput.
When a stream gets too backed up, batch sizes can balloon and batch latencies can introduce significant delays over time. If a stream is not monitored properly, data may be purged from kafka before it can be ingested.

Essentially, multiplex ingestion is a low cost buffer and built in safety net that allows for all streaming data to be processed more efficiently.

Quando um fluxo fica muito congestionado, o tamanho dos lotes pode aumentar e as latências dos lotes podem introduzir atrasos significativos ao longo do tempo.

<img src="ADE/image-61.png" width="40%" />

## Data Quality Enforcement Patterns
## Data Modeling
## Streaming joins and Startefulness

<img src="ADE/image-62.png" width="50%" />

<img src="ADE/image-63.png" width="50%" />

<img src="ADE/image-64.png" width="40%" />

<img src="ADE/image-65.png" width="40%" />

### Backfills

<img src="ADE/image-66.png" width="40%" />

## LAB 2.2.  Auto Load Data to Bronze

<img src="https://files.training.databricks.com/images/ade/ADE_arch_bronze.png" width="60%" />

```python
import dlt
import pyspark.sql.functions as F

source = spark.conf.get("source")
lookup_db = spark.conf.get("lookup_db")
```
[See DLT properties](https://docs.databricks.com/aws/en/dlt/properties)
``` python
@dlt.table(
    table_properties={
        "pipelines.reset.allowed": "false"
    }
)
def date_lookup():
    return spark.read.table(f"{lookup_db}.date_lookup").select("date", "week_part")
```
```python 
@dlt.table(
    partition_cols=["topic", "week_part"],
    table_properties={
        "quality": "bronze",
        "pipelines.reset.allowed": "false"
    }
)
def bronze():
    return (
    spark.readStream
        .format("cloudFiles")
        .schema("key BINARY, value BINARY, topic STRING, partition LONG, offset LONG, timestamp LONG")
        .option("cloudFiles.format", "json")
        .load(f"{source}/daily")
        .join(
        F.broadcast(dlt.read("date_lookup")), 
        F.to_date((F.col("timestamp")/1000).cast("timestamp")) == F.col("date"), "left")
    )
```
```python 
@dlt.table
def distinct_topics():
    return dlt.read("bronze").select("topic").distinct()
```

## LAB 2.3.1. Data Quality Enforcement

**Flagging**

To avoid multiple writes and managing multiple tables, you may choose to implement a flagging system to warn about violations while avoiding job failures. Flagging is a low touch solution with little overhead. These flags can easily be leveraged by filters in downstream queries to isolate bad data. **`case`** / **`when`** logic makes this easy.
```python
rules = {
  "valid_heartrate": "heartrate IS NOT NULL",
  "valid_device_id": "device_id IS NOT NULL",
  "valid_device_id_range": "device_id > 110000"
}

@dlt.table(
    table_properties={"quality": "silver"}
)
@dlt.expect_all_or_drop(rules)
def bpm_silver():
    return (
        dlt.read_stream("bpm_bronze")
          .select("*", F.when(F.col("heartrate") <= 0, "Negative BPM").otherwise("OK").alias("bpm_check"))
          .withWatermark("time", "30 seconds")
          .dropDuplicates(["device_id", "time"])
    )
```

**Quarantining**

The idea of quarantining is that bad records will be written to a separate location. This allows good data to processed efficiently, while additional logic and/or manual review of erroneous records can be defined and executed away from the main pipeline. Assuming that records can be successfully salvaged, they can be easily backfilled into the silver table they were deferred from.

For simplicity, we won't check for duplicate records as we insert data into the quarantine table.

```python
quarantine_rules = {}
quarantine_rules["invalid_record"] = f"NOT({' AND '.join(rules.values())})"
# {'invalid_record': 'NOT(heartrate IS NOT NULL AND device_id IS NOT NULL AND device_id > 110000)'}

@dlt.table
@dlt.expect_all_or_drop(quarantine_rules)
def bpm_quarantine():
    return (
        dlt.read_stream("bpm_bronze")
        )
```
## LAB 2.4.1
[Optimize stateful processing in DLT with watermarks](https://docs.databricks.com/aws/en/dlt/stateful-processing?language=Python#deduplicate-streaming-records)

Step 1: Stream Workouts from Multiplex Bronze


Stream records for the **`workout`** topic from the multiplex bronze table to create a **`workouts_bronze`** table.
1. Start a stream against the **`bronze`** table
1. Filter all records by **`topic = 'workout'`**
1. Parse and flatten JSON fields

```python
workouts_schema = "user_id INT, workout_id INT, timestamp FLOAT, action STRING, session_id INT"

@dlt.table(
    table_properties={"quality": "bronze"}
)
def workouts_bronze():
    return (
        dlt.read_stream("bronze")
          .filter("topic = 'workout'")
          .select(F.from_json(F.col("value").cast("string"), workouts_schema).alias("v"))
          .select("v.*")
        )
```
Step 2: Promote Workouts to Silver

```python
rules = {
  "valid_user_id": "user_id IS NOT NULL",
  "valid_workout_id": "workout_id IS NOT NULL"
}

@dlt.table(
    table_properties={"quality": "silver"}
)
@dlt.expect_all_or_drop(rules)
def workouts_silver():
    return (
        dlt.read_stream("workouts_bronze")
          .select("user_id", "workout_id", F.col("timestamp").cast("timestamp").alias("time"), "action", "session_id")
          .withWatermark("time", "30 seconds")
          .dropDuplicates(["user_id", "time"])
    )
```
Step 3: Quarantine Invalid Records

Implement a **`workouts_quarantine`** table for invalid records from **`workouts_bronze`**.

```python
quarantine_rules = {"invalid_record": f"NOT({' AND '.join(rules.values())})"}

@dlt.table
@dlt.expect_all_or_drop(quarantine_rules)
def workouts_quarantine():
    return (
        dlt.read_stream("workouts_bronze")    
    )
```
## LAB 2.5. Data Modeling - SCD Type 2

* [The APPLY CHANGES APIs: Simplify change data capture with DLT](https://docs.databricks.com/aws/en/dlt/cdc)
* [Use Delta Lake change data feed on Databricks](https://docs.databricks.com/aws/en/delta/delta-change-data-feed)

```python
import dlt
import pyspark.sql.functions as F
```
```python
users_schema = "user_id LONG, update_type STRING, timestamp FLOAT, dob STRING, sex STRING, gender STRING, first_name STRING, last_name STRING, address STRUCT<street_address: STRING, city: STRING, state: STRING, zip: INT>"    

@dlt.table(
    table_properties={"quality": "bronze"}
)
def users_cdc_bronze():
    return (
        dlt.read_stream("bronze")
          .filter("topic = 'user_info'")
          .select(F.from_json(F.col("value").cast("string"), users_schema).alias("v"))
          .select("v.*")
    )
```
```python
rules = {
  "valid_user_id": "user_id IS NOT NULL",
  "valid_operation": "update_type IS NOT NULL"
}

@dlt.table(
    table_properties={"quality":"bronze"}
)
@dlt.expect_all_or_drop(rules)
def users_cdc_clean():
    return (
        dlt.read_stream("users_cdc_bronze")        
            .select(
                # Adding a pseudonymized key to incremental workloads is as simple as adding a transformation.
#                 F.sha2(F.concat(F.col("user_id"), F.lit(salt)), 256).alias("alt_id"),
                F.col("user_id"),              
                F.col("timestamp").cast("timestamp").alias("updated"),
                F.to_date("dob", "MM/dd/yyyy").alias("dob"), 
                "sex", "gender", "first_name", "last_name", "address.*", "update_type")
    )

quarantine_rules = {}
quarantine_rules["invalid_record"] = f"NOT({' AND '.join(rules.values())})"

@dlt.table
@dlt.expect_all_or_drop(quarantine_rules)
def users_cdc_quarantine():
    return (
        dlt.read_stream("users_cdc_bronze")
        )
```
```python
# Process user updates from CDC feed
dlt.create_streaming_live_table(
  name="users_silver", 
  table_properties={"quality": "silver"}
)
dlt.apply_changes(
  target = "users_silver", 
  source = "users_cdc_clean",
  keys = ["user_id"], 
  sequence_by = F.col("updated"),
  apply_as_deletes = F.expr("update_type = 'delete'"),
  except_column_list = ["update_type"]
)
```
```python
dlt.create_streaming_live_table(
  name="SCD2_users", 
  table_properties={"quality": "silver"}
)
dlt.apply_changes(
  target = "SCD2_users", 
  source = "users_cdc_clean",
  keys = ["user_id"],
  sequence_by = F.col("updated"),
  apply_as_deletes = F.expr("update_type = 'delete'"),
  except_column_list = ["update_type"],
  stored_as_scd_type = "2" #Enable SCD2 and store individual updates
)
```
```python
rules = {
  "pk_must_be_unique": "number_of_records_for_key = 1"
}

@dlt.table(
    comment="Check that users table only contains unique user id"
)
@dlt.expect_all_or_fail(rules)
def unique_user_id():
    return spark.sql("""
      SELECT user_id, count(*) AS number_of_records_for_key
      FROM LIVE.users_silver
      GROUP BY user_id
    """)
```

## LAB 2.6. Streaming Joins

* [Work with joins on Databricks](https://docs.databricks.com/aws/en/transform/join)
* [Batches joins *stateless*](https://docs.databricks.com/aws/en/sql/language-manual/sql-ref-syntax-qry-select-join)
* For stream-stream joins:
    - [What is stateful streaming?](https://docs.databricks.com/aws/en/structured-streaming/stateful-streaming)
    - [Apply watermarks to control data processing thresholds](https://docs.databricks.com/aws/en/structured-streaming/watermarks)
    - [**Stream-Stream joins**](https://docs.databricks.com/aws/en/structured-streaming/examples#stream-stream-joins)

Very usefull to navegate from Silver to Gold.

Note that we will only be streaming from **one** of our tables. The **`completed_workouts`** table is no longer streamable as it breaks the requirement of an ever-appending source for Structured Streaming. However, when performing a stream-static join with a Delta table, each batch will confirm that the newest version of the static Delta table is being used.

```python
import dlt
import pyspark.sql.functions as F

source = spark.conf.get("source")
lookup_db = spark.conf.get("lookup_db")
```
```python
@dlt.table
def completed_workouts():
    return spark.sql(f"""
      SELECT a.user_id, a.workout_id, a.session_id, a.start_time start_time, b.end_time end_time, a.in_progress AND (b.in_progress IS NULL) in_progress
      FROM (
        SELECT user_id, workout_id, session_id, time start_time, null end_time, true in_progress
        FROM LIVE.workouts_silver
        WHERE action = "start") a
      LEFT JOIN (
        SELECT user_id, workout_id, session_id, null start_time, time end_time, false in_progress
        FROM LIVE.workouts_silver
        WHERE action = "stop") b
      ON a.user_id = b.user_id AND a.session_id = b.session_id
    """)
```
Sample Data of 

user_id	workout_id	time	action	session_id

23250	48	2019-12-01T07:32:16.000+00:00	start	452

12474	0	2019-12-01T17:44:32.000+00:00	stop	1

28588	34	2019-12-01T13:41:20.000+00:00	start	1

29213	43	2019-12-01T10:31:28.000+00:00	stop	294

27306	27	2019-12-01T08:00:00.000+00:00	stop	161

27306	16	2019-12-01T09:01:52.000+00:00	stop	162

24018	33	2019-12-01T07:38:40.000+00:00	stop	147

26847	14	2019-12-01T13:49:52.000+00:00	stop	2

40872	8	2019-12-01T21:05:04.000+00:00	stop	76

26847	6	2019-12-01T12:18:08.000+00:00	start	1

40872	8	2019-12-01T19:48:16.000+00:00	start	76

28776	4	2019-12-01T13:54:08.000+00:00	start	389



**Perform Stream-Static Join to Align Workouts to Heart Rate Recordings**

Below we'll configure our query to join our stream to our **`completed_workouts`** table. 

Note that our heart rate recordings only have **`device_id`**, while our workouts use **`user_id`** as the unique identifier. We'll need to use our **`user_lookup`** table to match these values. Because all tables are Delta Lake tables, we're guaranteed to get the latest version of each table during each microbatch transaction.

**NOTE**: The setup script includes logic to define a **`user_lookup`** table required for the join below.

Importantly, our devices occasionally send messages with negative recordings, which represent a potential error in the recorded values. We'll need to define predicate conditions to ensure that only positive recordings are processed.

```python
@dlt.table
def user_lookup():
    return spark.read.table(f"{lookup_db}.user_lookup")

# Stream static join
@dlt.table
def workout_bpm():
    return spark.sql("""
      SELECT d.user_id, d.workout_id, d.session_id, time, heartrate
      FROM STREAM(LIVE.bpm_silver) c
      INNER JOIN (
        SELECT a.user_id, b.device_id, workout_id, session_id, start_time, end_time
        FROM LIVE.completed_workouts a
        INNER JOIN LIVE.user_lookup b
        ON a.user_id = b.user_id) d
      ON c.device_id = d.device_id AND time BETWEEN start_time AND end_time
      WHERE c.bpm_check = 'OK'
    """)
```


# 3. Data Privacy and Governance Patterns

## Store Data Securely

PII - Personal Identifiable Information

<img src="ADE/image-67.png" width="40%" />

<img src="ADE/image-68.png" width="40%" />

<img src="ADE/image-69.png" width="40%" />

<img src="ADE/image-70.png" width="40%" />

<img src="ADE/image-71.png" width="40%" />

<img src="ADE/image-72.png" width="40%" />

<img src="ADE/image-73.png" width="40%" />

<img src="ADE/image-74.png" width="40%" />

<img src="ADE/image-75.png" width="40%" />

<img src="ADE/image-76.png" width="40%" />

<img src="ADE/image-77.png" width="40%" />

<img src="ADE/image-78.png" width="40%" />

<img src="ADE/image-79.png" width="40%" />

<img src="ADE/image-80.png" width="40%" />

## Streaming Data and Change Data Feed

Apply changes into - magic world!

<img src="ADE/image-81.png" width="40%" />

<img src="ADE/image-82.png" width="40%" />

<img src="ADE/image-83.png" width="40%" />

<img src="ADE/image-84.png" width="40%" />

Regarding materialized views, there's a need to capture an aggregated view of the gold level data for a dashboard or real time application. No need for re-aggregations off of full tables, while ensuring that changes are reflected appropriately.

<img src="ADE/image-85.png" width="40%" />

<img src="ADE/image-86.png" width="40%" />

## Deleting Data in the Lakehouse

<img src="ADE/image-87.png" width="40%" />

<img src="ADE/image-88.png" width="40%" />

<img src="ADE/image-89.png" width="40%" />

<img src="ADE/image-90.png" width="40%" />

<img src="ADE/image-91.png" width="40%" />

# Performance Optimization with Spark and Delta Lake

## Designing the Foundation

<img src="ADE/image-92.png" width="40%" />

<img src="ADE/image-93.png" width="40%" />

<img src="ADE/image-94.png" width="40%" />

z-order first 32 cols.

<img src="ADE/image-95.png" width="40%" />

<img src="ADE/image-96.png" width="40%" />

<img src="ADE/image-97.png" width="40%" />

<img src="ADE/image-98.png" width="40%" />

## Code Optimization

<img src="ADE/image-99.png" width="40%" />

<img src="ADE/image-100.png" width="40%" />

## Assess and Debug Spark Applications

<img src="ADE/image-101.png" width="40%" />

<img src="ADE/image-102.png" width="40%" />

<img src="ADE/image-103.png" width="40%" />

<img src="ADE/image-104.png" width="40%" />

<img src="ADE/image-105.png" width="40%" />

<img src="ADE/image-106.png" width="40%" />

<img src="ADE/image-107.png" width="40%" />

## Spark UI Simulator

<img src="ADE/image-108.png" width="40%" />

## Shuffles

<img src="ADE/image-109.png" width="40%" />

<img src="ADE/image-110.png" width="40%" />

<img src="ADE/image-111.png" width="40%" />

<img src="ADE/image-112.png" width="40%" />

AQE - Adaptive Query Execution

CBO - cost-based optimizer

<img src="ADE/image-114.png" width="40%" />

<img src="ADE/image-113.png" width="40%" />

<img src="ADE/image-115.png" width="40%" />

## Spill

<img src="ADE/image-116.png" width="40%" />

<img src="ADE/image-117.png" width="40%" />

<img src="ADE/image-118.png" width="40%" />

<img src="ADE/image-119.png" width="40%" />

## Skew

<img src="ADE/image-120.png" width="40%" />

<img src="ADE/image-121.png" width="40%" />

<img src="ADE/image-122.png" width="40%" />

<img src="ADE/image-123.png" width="40%" />

## Serialization

<img src="ADE/image-124.png" width="40%" />

<img src="ADE/image-125.png" width="40%" />

<img src="ADE/image-126.png" width="40%" />

## Fine-Tuning: Choosing the Right

<img src="ADE/image-127.png" width="40%" />

<img src="ADE/image-128.png" width="40%" />

<img src="ADE/image-129.png" width="40%" />

<img src="ADE/image-130.png" width="40%" />

<img src="ADE/image-131.png" width="40%" />

<img src="ADE/image-132.png" width="40%" />

# 5. Softwares Engineer Practices for DLT Pipelines

CI/CD for DLT 

<img src="ADE/image-133.png" width="40%" />

<img src="ADE/image-134.png" width="40%" />

<img src="ADE/image-135.png" width="40%" />

<img src="ADE/image-136.png" width="40%" />

# 6. Automate Prouction Workflows

## Introduction to the REST API and CLI

<img src="ADE/image-137.png" width="40%" />

<img src="ADE/image-138.png" width="40%" />

<img src="ADE/image-139.png" width="40%" />

<img src="ADE/image-140.png" width="40%" />

<img src="ADE/image-141.png" width="40%" />


## 1st step: Generate Tokens

### A - Verify that Personal Access Token are enabled in the workspace

1. Your Name >> Settings >> Developer >> Access tokens: Manage
<img src="ADE/image-142.png" width="40%" />

2. Generate new token:
    Comment => put a identification or comment to specify the new token
    Lifetime (days) =>  between 1 and 730

<img src="ADE/image-143.png" width="40%" />

3. Copy the token! >> Done
<img src="ADE/image-144.png" width="40%" />


4. If you wish, you can revoke your token:

<img src="ADE/image-145.png" width="40%" />

## 2nd - Setup CLI in Cluster Web Terminal

1. View >> Web Terminal
<img src="ADE/image-146.png" width="40%" />

1. Then, it open a terminal. Inside terminal type...
<img src="ADE/image-147.png" width="40%" />

1. Install the CLI
    ```
    pip install databricks-cli
    ```
1. Set up authentication info and host to connect to
    ```
    databricks configure --host {db_instance} --token
    ```
        db_instance => url: https: ..... .com
        ex.: https://dbc-cfea1dab-ea1b.cloud.databricks.com/
1. This isssues a prompt ( Token: ) to enter your personal token
    ```
    { db_token}
    ```
1. Verify authentication to CLI
    ```
    databricks workspace ls /Users/labuser9417981_1743249834@vocareum.com
    ```

Optionally, store your secrets in a secret scope using th Databricks CLI:

    
    databricks secrets create-scope --scope "<scope-name>"

    databricks secrets put --scope *scope-name* --key *password1*
    

isso vai abrir o editor Vim.
1. Esc para sair do modo de edição
1. :wq para escrever e sair.
    <img src="ADE/image-148.png" width="40%" />

    for update a key, just do the same process ( ```databricks secrets put ...```)
    Recover the password1:
    ```
    password1 = dbutils.secrets.get(scope = "scope-name", key = "password1")
    ```
    list the scope:
    databricks secrets list-scopes

    list the keys:
    databricks secrets list --scopes projeto

<img src="ADE/image-149.png" width="40%" />

    Remove scope:

    databricks secrets delete-scope --scope name

    Remove key:

    databricks secrets delete --scope name --key name

## Deploy Batch and Streaming Jobs

<img src="ADE/image-150.png" width="40%" />

<img src="ADE/image-151.png" width="40%" />

<img src="ADE/image-152.png" width="40%" />

## Using the Databricks API

curl:
<img src="ADE/image-153.png" width="40%" />

list of pipelines... json format

python:

vim test.py
crl+v
diw  (delete and insertthe word)
i =>  insert
<img src="ADE/image-154.png" width="40%" />

ESC >> :wq

terminal: # python test.py



## Deploy a Pipeline with the Databricks CLI

Be sure to have your credentials already in Databricks CLI (see: ```Setup CLI in Cluster Web Terminal```):
```
databricks configure --host https://dbc-cfea1dab-ea1b.cloud.databricks.com/ --token
```

Deploy:
```
databricks pipelines start-update $DATABRICKS_PIPELINE_ID --full-refresh
```

Or trigger a pipeline update


Get status:
```
databricks pipelines get $DATABRICKS_PIPELINE_ID
```

Get seeting of a pipeline (We can push our ```settings.json``` for version control, for exemple, and to put inside )

```
cat ./var/settings.json
```

Create a new pipeline based on the settings.json file

```
databricks pipelines create --json @var/settings.json
```
Delete the pipeline

```
databricks pipelines delete "pipeline_id"
```

## Terraform

<img src="ADE/image-155.png" width="40%" />

<img src="ADE/image-156.png" width="40%" />

<img src="ADE/image-157.png" width="40%" />

<img src="ADE/image-158.png" width="40%" />

<img src="ADE/image-159.png" width="40%" />