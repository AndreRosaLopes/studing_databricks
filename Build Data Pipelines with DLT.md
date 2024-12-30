
# DLT

The advantages of using a Delta Live Tables (DLT) pipeline over a traditional ETL pipeline in Databricks:
*	DLT has built-in quality controls and data quality monitoring.
*	DLT provides granular observability into pipeline operations and automatic error handling.


### Esunre correctness with Expectations

```sql
CONTRAINT valid_timestamp
EXPECT (timestamp > '20212-01-01')
ON VIOLATION DROP
```
Decorator on python:
```python
@dlt.expect_or_drop(
    "valid_timestamps",
    col("timestamp">'2012-01-01')
)
```
Possibles (flexible) policies:
* Track number of bad records
* Drop bad records
* Abort processing for a single bad record

### Operations

### Using Spark Structured Streaming for ingestion

```sql
CREATE STREAMING LIVE TABLE raw_data
AS SELECT *
FROM cloud_files("/data", "json")
```
From any Delta Live Table (it must be append-only source):
```sql
CREATE STREAMING LIVE TABLE mystream
AS SELECT *
FROM STREAM(my_table)
```

Modularize your code with configuration.
A pipeline's configuration is a map of key value pairs that can be used to parameterize your code:

* Improve code readability/maintainability
* Reuse code in multiple pipelines for different data

```sql
CREATE STREAMING LIVE TABLE raw_data
AS SELECT *
FROM cloud_files("${my_etl.input.path}", "json")
```

```python
@dlt.table
def data ()
    input_path = spark.conf.get("my_etl.input.path")
    spark.readStream.format("cloud_files").load(input_path)
```

### CDC


### Automated Data Management (DLT)

## SQL Lab

Using Delta Live Tables (DLT) to process raw data from JSON files landing in cloud object storage through a series of tables to drive analytic workloads in the lakehouse.

* The bronze table contains raw records loaded from JSON enriched with data describing how records were ingested
* The silver table validates and enriches the fields of interest
* The gold table contains aggregate data to drive business insights and dashboarding

There are two distinct types of persistent tables that can be created with DLT:

* Materialized View are materialized views for the lakehouse; they will return the current results of any query with each refresh
* Streaming Tables are designed for incremental, near-real time data processing

The **`cloud_files()`** method enables Auto Loader to be used natively with SQL.

### Bronze

```sql
CREATE OR REFRESH STREAMING TABLE orders_bronze
AS SELECT current_timestamp() processing_time, input_file_name() source_file, *
FROM cloud_files("${source}/orders", "json", map("cloudFiles.inferColumnTypes", "true"))
```
Note that the code below omits the Auto Loader option to infer schema. When data is ingested from JSON without the schema provided or inferred, fields will have the correct names but will all be stored as **`STRING`** type.

```sql
CREATE OR REFRESH STREAMING TABLE customers_bronze
COMMENT "Raw data from customers CDC feed"
AS SELECT current_timestamp() processing_time, input_file_name() source_file, *
FROM cloud_files("${source}/customers", "json")
```
```sql
CREATE STREAMING TABLE customers_bronze_clean
(CONSTRAINT valid_id EXPECT (customer_id IS NOT NULL) ON VIOLATION FAIL UPDATE,
CONSTRAINT valid_operation EXPECT (operation IS NOT NULL) ON VIOLATION DROP ROW,
CONSTRAINT valid_name EXPECT (name IS NOT NULL or operation = "DELETE"),
CONSTRAINT valid_address EXPECT (
  (address IS NOT NULL and 
  city IS NOT NULL and 
  state IS NOT NULL and 
  zip_code IS NOT NULL) or
  operation = "DELETE"),
CONSTRAINT valid_email EXPECT (
  rlike(email, '^([a-zA-Z0-9_\\-\\.]+)@([a-zA-Z0-9_\\-\\.]+)\\.([a-zA-Z]{2,5})$') or -- regex pattern
  operation = "DELETE") ON VIOLATION DROP ROW)
AS SELECT *
  FROM STREAM(LIVE.customers_bronze)
```
### Silver

```sql
CREATE OR REFRESH STREAMING TABLE orders_silver
(CONSTRAINT valid_date EXPECT (order_timestamp > "2021-01-01") ON VIOLATION FAIL UPDATE)
COMMENT "Append only orders with valid timestamps"
TBLPROPERTIES ("quality" = "silver")
AS SELECT timestamp(order_timestamp) AS order_timestamp, * EXCEPT (order_timestamp, source_file, _rescued_data)
FROM STREAM(LIVE.orders_bronze)
```
```sql
CREATE OR REFRESH STREAMING TABLE customers_silver;

APPLY CHANGES INTO LIVE.customers_silver
  FROM STREAM(LIVE.customers_bronze_clean)
  KEYS (customer_id)
  APPLY AS DELETE WHEN operation = "DELETE"
  SEQUENCE BY timestamp
  COLUMNS * EXCEPT (operation, source_file, _rescued_data)
```
Note:
* The **`LIVE`** keyword is used in place of the schema name to refer to the target schema configured for the current DLT pipeline
* The **`STREAM`** method allows users to declare a streaming data source for SQL queries

 **Live Tables**

* Always "correct", meaning their contents will match their definition after any update.
* Return same results as if table had just been defined for first time on all data.
* Should not be modified by operations external to the DLT Pipeline (you'll either get undefined answers or your change will just be undone).

 **Streaming Tables**

* Only supports reading from "append-only" streaming sources.
* Only reads each input batch once, no matter what (even if joined dimensions change, or if the query definition changes, etc).
* Can perform operations on the table outside the managed DLT Pipeline (append data, perform GDPR, etc).

### Gold

```sql
CREATE OR REFRESH LIVE TABLE orders_by_date
AS SELECT date(order_timestamp) AS order_date, count(*) AS total_daily_orders
FROM LIVE.orders_silver
GROUP BY date(order_timestamp)
```

```sql
CREATE LIVE TABLE customer_counts_state
  COMMENT "Total active customers per state"
AS SELECT state, count(*) as customer_count, current_timestamp() updated_at
  FROM LIVE.customers_silver
  GROUP BY state
```

## DLT Views

The query below defines a DLT view by replacing **`TABLE`** with the **`VIEW`** keyword.

Views in DLT differ from persisted tables, and can optionally be defined as **`STREAMING`**.

Views have the same update guarantees as live tables, but the results of queries are not stored to disk.

Unlike views used elsewhere in Databricks, DLT views are not persisted to the metastore, meaning that they can only be referenced from within the DLT pipeline they are a part of. (This is similar scoping to temporary views in most SQL systems.)

Views can still be used to enforce data quality, and metrics for views will be collected and reported as they would be for tables.

The results of live tables are stored to disk, while the results of views can only be referenced from within the DLT pipeline in which they are defined.

```sql
CREATE LIVE VIEW subscribed_order_emails_v
  AS SELECT a.customer_id, a.order_id, b.email 
    FROM LIVE.orders_silver a
    INNER JOIN LIVE.customers_silver b
    ON a.customer_id = b.customer_id
    WHERE notifications = 'Y'
```

# Python syntax
## About DLT Library Notebooks

DLT syntax is not intended for interactive execution in a notebook. This notebook will need to be scheduled as part of a DLT pipeline for proper execution. 
## Parameterization

During the configuration of the DLT pipeline, a number of options were specified. One of these was a key-value pair added to the **Configurations** field.

Configurations in DLT pipelines are similar to parameters in Databricks Jobs, but are actually set as Spark configurations.

In Python, we can access these values using **`spark.conf.get()`**.




  ```python
  import dlt
  import pyspark.sql.functions as F

  source = spark.conf.get("source")
  ```
There are two distinct types of persistent tables that can be created with DLT:
* **Materialized Views** are materialized views for the lakehouse; they will return the current results of any query with each refresh
* **Streaming tables** are designed for incremental, near-real time data processing

## Streaming Ingestion with Auto Loader

Databricks has developed the [Auto Loader](https://docs.databricks.com/ingestion/auto-loader/index.html) functionality to provide optimized execution for incrementally loading data from cloud object storage into Delta Lake. Using Auto Loader with DLT is simple: just configure a source data directory, provide a few configuration settings, and write a query against your source data. Auto Loader will automatically detect new data files as they land in the source cloud object storage location, incrementally processing new records without the need to perform expensive scans and recomputing results for infinitely growing datasets.

Auto Loader can be combined with Structured Streaming APIs to perform incremental data ingestion throughout Databricks by configuring the **`format("cloudFiles")`** setting. In DLT, you'll only configure settings associated with reading data, noting that the locations for schema inference and evolution will also be configured automatically if those settings are enabled.

The query below returns a streaming DataFrame from a source configured with Auto Loader.

In addition to passing **`cloudFiles`** as the format, here we specify:
* The option **`cloudFiles.format`** as **`json`** (this indicates the format of the files in the cloud object storage location)
* The option **`cloudFiles.inferColumnTypes`** as **`True`** (to auto-detect the types of each column)
* The path of the cloud object storage to the **`load`** method
* A select statement that includes a couple of **`pyspark.sql.functions`** to enrich the data alongside all the source fields

By default, **`@dlt.table`** will use the name of the function as the name for the target table.

``` python
@dlt.table
def orders_bronze():
    return (
        spark.readStream
            .format("cloudFiles")
            .option("cloudFiles.format", "json")
            .option("cloudFiles.inferColumnTypes", True)
            .load(f"{source}/orders")
            .select(
                F.current_timestamp().alias("processing_time"), 
                F.input_file_name().alias("source_file"), 
                "*"
            )
    )
```

## Validating, Enriching, and Transforming Data

DLT allows users to easily declare tables from results of any standard Spark transformations. DLT adds new functionality for data quality checks and provides a number of options to allow users to enrich the metadata for created tables.

Let's break down the syntax of the query below.

### Options for **`@dlt.table()`**

There are <a href="https://docs.databricks.com/data-engineering/delta-live-tables/delta-live-tables-python-ref.html#create-table" target="_blank">a number of options</a> that can be specified during table creation. Here, we use two of these to annotate our dataset.

##### **`comment`**

Table comments are a standard for relational databases. They can be used to provide useful information to users throughout your organization. In this example, we write a short human-readable description of the table that describes how data is being ingested and enforced (which could also be gleaned from reviewing other table metadata).

##### **`table_properties`**

This field can be used to pass any number of key/value pairs for custom tagging of data. Here, we set the value **`silver`** for the key **`quality`**.

Note that while this field allows for custom tags to be arbitrarily set, it is also used for configuring number of settings that control how a table will perform. While reviewing table details, you may also encounter a number of settings that are turned on by default any time a table is created.

### Data Quality Constraints

The Python version of DLT uses decorator functions to set <a href="https://docs.databricks.com/data-engineering/delta-live-tables/delta-live-tables-expectations.html#delta-live-tables-data-quality-constraints" target="_blank">data quality constraints</a>. We'll see a number of these throughout the course.

DLT uses simple boolean statements to allow quality enforcement checks on data. In the statement below, we:
* Declare a constraint named **`valid_date`**
* Define the conditional check that the field **`order_timestamp`** must contain a value greater than January 1, 2021
* Instruct DLT to fail the current transaction if any records violate the constraint by using the decorator **`@dlt.expect_or_fail()`**

Each constraint can have multiple conditions, and multiple constraints can be set for a single table. In addition to failing the update, constraint violation can also automatically drop records or just record the number of violations while still processing these invalid records.

### DLT Read Methods

The Python **`dlt`** module provides the **`read()`** and **`read_stream()`** methods to easily configure references to other tables and views in your DLT Pipeline. This syntax allows you to reference these datasets by name without any database reference. You can also use **`spark.table("LIVE.<table_name.")`**, where **`LIVE`** is a keyword substituted for the database being referenced in the DLT Pipeline.
```python
@dlt.table(
    comment = "Append only orders with valid timestamps",
    table_properties = {"quality": "silver"})
@dlt.expect_or_fail("valid_date", F.col("order_timestamp") > "2021-01-01")
def orders_silver():
    return (
        dlt.read_stream("orders_bronze")
            .select(
                "processing_time",
                "customer_id",
                "notifications",
                "order_id",
                F.col("order_timestamp").cast("timestamp").alias("order_timestamp")
            )
    )
```
## Materialized View vs. Streaming Tables

The two functions we've reviewed so far have both created streaming tables. Below, we see a simple function that returns a live table (or materialized view) of some aggregated data.

Spark has historically differentiated between batch queries and streaming queries. Live tables and streaming tables have similar differences.

Note that these table types inherit the syntax (as well as some of the limitations) of the PySpark and Structured Streaming APIs.

Below are some of the differences between these types of tables.

### Materialized View
* Always "correct", meaning their contents will match their definition after any update.
* Return same results as if table had just been defined for first time on all data.
* Should not be modified by operations external to the DLT Pipeline (you'll either get undefined answers or your change will just be undone).

### Streaming Tables
* Only supports reading from "append-only" streaming sources.
* Only reads each input batch once, no matter what (even if joined dimensions change, or if the query definition changes, etc).
* Can perform operations on the table outside the managed DLT Pipeline (append data, perform GDPR, etc).

```python
@dlt.table(
    comment = "Append only orders with valid timestamps",
    table_properties = {"quality": "silver"})
@dlt.expect_or_fail("valid_date", F.col("order_timestamp") > "2021-01-01")
def orders_silver():
    return (
        dlt.read_stream("orders_bronze")
            .select(
                "processing_time",
                "customer_id",
                "notifications",
                "order_id",
                F.col("order_timestamp").cast("timestamp").alias("order_timestamp")
            )
    )
```

Note that the code below omits the Auto Loader option to infer schema. When data is ingested from JSON without the schema provided or inferred, fields will have the correct names but will all be stored as **`STRING`** type.

```python
@dlt.table(
    name = "customers_bronze",
    comment = "Raw data from customers CDC feed"
)
def ingest_customers_cdc():
    return (
        spark.readStream
        .format("cloudFiles")
        .option("cloudFiles.format", "json")
        .load(f"{source}/customers")
        .select(
            F.current_timestamp().alias("processing_time"),
            F.input_file_name().alias("source_file"),
            "*"
        )
    )
```

### Quality Enforcement Continued

The query below demonstrates:
* The 3 options for behavior when constraints are violated
* A query with multiple constraints
* Multiple conditions provided to one constraint
* Using a built-in SQL function in a constraint

About the data source:
* Data is a CDC feed that contains **`INSERT`**, **`UPDATE`**, and **`DELETE`** operations. 
* Update and insert operations should contain valid entries for all fields.
* Delete operations should contain **`NULL`** values for all fields other than the timestamp, **`customer_id`**, and operation fields.

```python
@dlt.table
@dlt.expect_or_fail("valid_id", "customer_id IS NOT NULL")
@dlt.expect_or_drop("valid_operation", "operation IS NOT NULL")
@dlt.expect("valid_name", "name IS NOT NULL or operation = 'DELETE'")
@dlt.expect("valid_adress", """
    (address IS NOT NULL and 
    city IS NOT NULL and 
    state IS NOT NULL and 
    zip_code IS NOT NULL) or
    operation = "DELETE"
    """)
@dlt.expect_or_drop("valid_email", """
    rlike(email, '^([a-zA-Z0-9_\\\\-\\\\.]+)@([a-zA-Z0-9_\\\\-\\\\.]+)\\\\.([a-zA-Z]{2,5})$') or 
    operation = "DELETE"
    """)
def customers_bronze_clean():
    return (
        dlt.read_stream("customers_bronze")
    )
```
## Processing CDC Data with **`dlt.apply_changes()`**

DLT introduces a new syntactic structure for simplifying CDC feed processing.

**`dlt.apply_changes()`** has the following guarantees and requirements:
* Performs incremental/streaming ingestion of CDC data
* __Provides simple syntax to specify one or many fields as the primary key for a table__
* __Default assumption is that rows will contain inserts and updates__
* Can optionally apply deletes
* Automatically orders late-arriving records using user-provided sequencing field
* Uses a simple syntax for specifying columns to ignore with the **`except_column_list`**
* Will default to applying changes as Type 1 SCD

```python
dlt.create_target_table(
    name = "customers_silver")

dlt.apply_changes(
    target = "customers_silver",
    source = "customers_bronze_clean",  # --> note that the source must be append-only
    keys = ["customer_id"],
    sequence_by = F.col("timestamp"),    # --> in order that should be applied
    apply_as_deletes = F.expr("operation = 'DELETE'"),
    except_column_list = ["operation", "source_file", "_rescued_data"])
```
## Querying Tables with Applied Changes

**`dlt.apply_changes()`** defaults to creating a Type 1 SCD table, meaning that each unique key will have at most 1 record and that updates will overwrite the original information.

While the target of our operation in the previous cell was defined as a streaming table, data is being updated and deleted in this table (and so breaks the append-only requirements for streaming table sources). As such, downstream operations cannot perform streaming queries against this table. 

This pattern ensures that if any updates arrive out of order, downstream results can be properly recomputed to reflect updates. It also ensures that when records are deleted from a source table, these values are no longer reflected in tables later in the pipeline.

Below, we define a simple aggregate query to create a live table from the data in the **`customers_silver`** table.