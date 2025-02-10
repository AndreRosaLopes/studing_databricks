# Delta Lake and Data Objects 

What is Delta Lake?
* Delta Lake is an open-source data management platform
* It enhances data operations and analytics
* It leverages standard data formats
* Optimized for cloud object storage
* Built for efficient metadata handling

The Delta Lake Architecture

![alt text](Data_Ingestion_with_Delta_Lake\Architecture.png)

![alt text](Data_Ingestion_with_Delta_Lake\image-6.png)

Key Features of Delta Lake

* *Upadade and Delete:* Delta Lake allows for the modification and removal of records, offering a crucial distinction from other data formats.
* *Data Skipping Indes:* Delta Lake employs file statistics to optimize query performance by skipping unnecessary data scans.

![alt text](Data Ingestion with Delta Lake\Medallion.png)

Delta Lake allows you to incrementally improve the quality of your data until is ready for consumption.

## Liquid Clustering
No more partitions
* Fast
    - Raster writes and similar reads vs.

    ![alt text](Data_Ingestion_with_Delta_Lake\image-7.png)

Scenarious

# LAB 01 - Set Up and Load Delta Tables

While different organizations may have varying policies for how data is initially loaded into Databricks, we typically recommend that early tables represent a mostly raw version of the data, and that validation and enrichment occur in later stages. This pattern ensures that even if data doesn't match expectations with regards to data types or column names, no data will be dropped, meaning that programmatic or manual intervention can still salvage data in a partially corrupted or invalid state.

Example of query
```sql
SELECT * 
FROM parquet.`/Volumes/dbacademy_ecommerce/v01/raw/sales-historical/` 
LIMIT 10;
```

## Create Table as Select (CTAS)
CTAS statements automatically infer schema information from query results and do not support manual schema declaration.

```sql
CREATE OR REPLACE TABLE historical_sales_bronze 
USING DELTA --not necessary
AS
SELECT * 
FROM parquet.`/Volumes/dbacademy_ecommerce/v01/raw/sales-historical/`;

```

By running `DESCRIBE <table-name>`, we can see column names and data types. We see that the schema of this table looks correct.

```sql
DESCRIBE historical_sales_bronze;
```

```sql
DROP TABLE IF EXISTS sales_bronze;

CREATE TABLE sales_bronze 
USING DELTA 
AS
SELECT * 
FROM read_files("/Volumes/dbacademy_ecommerce/v01/raw/sales-csv/",
      format => "csv",
      sep => "|",
      header => true,
      mode => "FAILFAST"); -- This will cause the statement to throw an error if there is any malformed data

```

DESCRIBE EXTENDED allow us to vizualize metadata of columns and their types and more: Delta Statics Columns and Detailed Table Information

```sql
DESCRIBE EXTENDED sales_bronze;
```

The [IDENTIFIER clause](https://docs.databricks.com/en/sql/language-manual/sql-ref-names-identifier-clause.html) interprets a constant string as a:
- table or view name
- function name
- column name
- field name
- schema name

```sql
DESCRIBE SCHEMA IDENTIFIER(DA.schema_name);
```

We will **not** be creating external tables in this course. You can learn about creating external tables [here](https://docs.databricks.com/en/sql/language-manual/sql-ref-external-tables.html).


## Load Incrementally
COPY INTO loads data from data files into a Delta table. This is a retriable and idempotent operation, meaning that files in the source location that have already been loaded are skipped.

**`COPY INTO`** provides SQL engineers an idempotent option to incrementally ingest data from external systems.

Note that this operation does have some expectations:
- Data schema should be consistent
- Duplicate records should try to be excluded or handled downstream

This operation is potentially much cheaper than full table scans for data that grows predictably.

We want to capture new data but not re-ingest files that have already been read. We can use `COPY INTO` to perform this action. 

The first step is to create an empty table. We can then use COPY INTO to infer the schema of our existing data and copy data from new files that were added since the last time we ran `COPY INTO`.

```sql
COPY INTO users_bronze
  FROM '/Volumes/dbacademy_ecommerce/v01/raw/users-30m/'
  FILEFORMAT = parquet
  COPY_OPTIONS ('mergeSchema' = 'true');
```
## Creating External Tables

External tables are tables whose data is stored outside of the managed storage location specified for the metastore, catalog, or schema. Use external tables only when you require direct access to the data outside of Databricks clusters or Databricks SQL warehouses.

In order to provide access to an external storage location, a user with the necessary privileges must follow the instructions found [here](https://docs.databricks.com/en/sql/language-manual/sql-ref-external-locations.html).

```sql
DROP TABLE IF EXISTS sales_csv;
CREATE TABLE sales_csv
(order_id LONG, email STRING, transactions_timestamp LONG, total_item_quantity INTEGER, purchase_revenue_in_usd DOUBLE, unique_items INTEGER, items STRING)
USING CSV
OPTIONS (
header = "true",
delimiter = "|"
)
LOCATION "path_to_the_pre-configured_external_location"
```

If we were defining tables or queries against external data sources, we **cannot** expect the performance guarantees associated with Delta Lake and the Data Intelligence Platform.

For example: While Delta Lake tables will guarantee that you always query the most recent version of your source data, tables registered against other data sources may represent older cached versions.

## Built-In Functions

Databricks has a vast [number of built-in functions](https://docs.databricks.com/en/sql/language-manual/sql-ref-functions-builtin.html) you can use in your code.

We are going to create a table for user data generated by the previous point-of-sale system, but we need to make some changes. 

The `first_touch_timestamp` is in the wrong format. We need to divide the timestamp that is currently in microseconds by 1e6 (1 million). We will then use `CAST` to cast the result to a [TIMESTAMP](https://docs.databricks.com/en/sql/language-manual/data-types/timestamp-type.html). Then, we `CAST` to [DATE](https://docs.databricks.com/en/sql/language-manual/data-types/date-type.html).

Since we want to make changes to the `first_touch_timestamp` data, we need to use the `CAST` keyword. The syntax for `CAST` is `CAST(column AS data_type)`. We first cast the data to a `TIMESTAMP` and then to a `DATE`.  To use `CAST` with `COPY INTO`, we need to use a `SELECT` clause (make sure you include the parentheses) after the word `FROM` (in the `COPY INTO`).

Our **`SELECT`** clause leverages two additional built-in Spark SQL commands useful for file ingestion:
* **`current_timestamp()`** records the timestamp when the logic is executed
* **`_metadata.file_name`** records the source data file for each record in the table

```sql
CREATE TABLE users_bronze;
COPY INTO users_bronze FROM
  (SELECT *, 
    cast(cast(user_first_touch_timestamp/1e6 AS TIMESTAMP) AS DATE) first_touch_date, 
    current_timestamp() updated,
    _metadata.file_name source_file
  FROM '/Volumes/dbacademy_ecommerce/v01/raw/users-historical/')
  FILEFORMAT = PARQUET
  COPY_OPTIONS ('mergeSchema' = 'true');
```

# LAB02 - Basic Transformations

## Cloning Delta Lake Tables
Delta Lake has two options for efficiently copying Delta Lake tables. In either case, data modifications applied to the cloned version of the table will be tracked and stored separately from the source. Cloning is a great way to set up tables for testing SQL code while still in development.

### 1. DEEP CLONE
**`DEEP CLONE`** fully copies data and metadata from a source table to a target. This copy occurs incrementally, so executing this command again can sync changes from the source to the target location.


**Because all the data files must be copied over, this can take quite a while for large datasets.**

```sql
CREATE OR REPLACE TABLE historical_sales_clone
DEEP CLONE historical_sales_bronze;
```
### 2. SHALLOW CLONE

If you wish to create a copy of a table quickly to test out applying changes without the risk of modifying the current table, **`SHALLOW CLONE`** can be a good option. **Shallow clones just copy the Delta transaction logs**, meaning that the data doesn't move.

```SQL
CREATE OR REPLACE TABLE historical_sales_shallow_clone
SHALLOW CLONE historical_sales_bronze;
```

## Complete Overwrites 
Complete Overwrites are atomically replace all of the data in a table. Benefits to overwriting tables instead of deleting and recreating tables:
- Overwriting a table is much faster because it doesn’t need to list the directory recursively or delete any files.
- The old version of the table still exists; can easily retrieve the old data using Time Travel.
- It’s an atomic operation. Concurrent queries can still read the table while you are deleting the table.
- Due to ACID transaction guarantees, if overwriting the table fails, the table will be in its previous state.

```sql
CREATE OR REPLACE TABLE events AS
SELECT * 
FROM parquet.`/Volumes/dbacademy_ecommerce/v01/raw/events-historical/`;
```

## INSERT OVERWRITE

- Can only overwrite an existing table
- Can overwrite only with new records that match the current table schema
- Can overwrite individual partitions

```sql
INSERT OVERWRITE events
SELECT * 
FROM parquet.`/Volumes/dbacademy_ecommerce/v01/raw/events-historical/`;
```

## Merge

Delta Lake supports inserts, updates and deletes in **`MERGE`**, and supports extended syntax beyond the SQL standards to facilitate advanced use cases.

```sql
MERGE INTO users a
USING users_update b
ON a.user_id = b.user_id
WHEN MATCHED AND a.email IS NULL AND b.email IS NOT NULL THEN
  UPDATE SET email = b.email, updated = b.updated
WHEN NOT MATCHED THEN 
  INSERT (user_id, email, updated)
  VALUES (b.user_id, b.email, b.updated);
```

```sql
--Insert-Only Merge for Deduplication
MERGE INTO events a
USING events_update b
ON a.user_id = b.user_id AND a.event_timestamp = b.event_timestamp
WHEN NOT MATCHED AND b.traffic_source = 'email' THEN 
  INSERT *;
```
### Merge with Schema Evolution

```sql
-- CRAS adding a collumn:
CREATE OR REPLACE TABLE purchase_dates (
  id STRING, 
  transaction_timestamp STRING, 
  price STRING,
  date DATE GENERATED ALWAYS AS (
    cast(cast(transaction_timestamp/1e6 AS TIMESTAMP) AS DATE))
    COMMENT "generated based on `transaction_timestamp` column");
```
```sql
-- You can set this globally. However, this will not work with Serverless.
SET spark.databricks.delta.schema.autoMerge.enabled=true;

MERGE INTO purchase_dates a
USING purchases b
ON a.id = b.id
WHEN NOT MATCHED THEN
  INSERT *;
```
Alternatively:
```sql
MERGE WITH SCHEMA EVOLUTION INTO purchase_dates a
USING purchases b
ON a.id = b.id
WHEN NOT MATCHED THEN
  INSERT *;
```

## Add a Table Constraint

Databricks can support standard SQL constraint management clauses to ensure the quality and integrity of data added to a table because Delta Lake enforces schema on write.

Databricks currently support two types of constraints:
* <a href="https://docs.databricks.com/delta/delta-constraints.html#not-null-constraint" target="_blank">**`NOT NULL`** constraints</a>
* <a href="https://docs.databricks.com/delta/delta-constraints.html#check-constraint" target="_blank">**`CHECK`** constraints</a>

Ensure that no data violating the constraint is already in the table prior to defining the constraint. Once a constraint has been added to a table, data violating the constraint will result in write failure.

```sql
ALTER TABLE purchase_dates 
  ADD CONSTRAINT valid_date CHECK (date > '2020-01-01');
```

# LAB04 - Cleaning Data

### Remove nulls

```sql
-- Remove nulls
CREATE OR REPLACE TABLE users_silver_working AS
SELECT * 
FROM users_silver_working 
WHERE user_id IS NOT NULL;
```

### Deduplicate 
```sql
INSERT OVERWRITE users_silver_working 
  SELECT DISTINCT(*) 
  FROM users_silver_working;
```
### Deduplicate Rows Based on Specific Columns.
we are using the aggregate function **`max`** as a hack to:
- Keep values from the **`email`** and **`updated`** columns in the result of our group by
- Capture non-null emails when multiple records are present

```sql
-- Deduplicate Rows Based on Specific Columns.
INSERT OVERWRITE users_silver_working
SELECT 
  user_id, 
  user_first_touch_timestamp, 
  max(email) AS email, 
  max(updated) AS updated 
FROM users_silver_working
WHERE user_id IS NOT NULL
GROUP BY user_id, user_first_touch_timestamp;
```

### Validate Datasets

Validate that the **`user_id`** for each row is unique.

```sql
SELECT max(row_count) = 1 AS no_duplicate_user_ids 
FROM (
  SELECT user_id, count(*) AS row_count
  FROM users_silver_working
  GROUP BY user_id
);
```
This will return **`true`** or **`false`**.

```sql
SELECT max(user_id_count) = 1 AS one_user_id_per_email 
FROM (
  SELECT email, count(user_id) AS user_id_count
  FROM users_silver_working
  WHERE email IS NOT NULL
  GROUP BY email
);
```

## [Date Format](https://spark.apache.org/docs/latest/sql-ref-datetime-pattern.html) and [Regex](https://www.w3schools.com/java/java_regex.asp)

```sql
INSERT INTO users_silver
(SELECT 
  user_id,
  user_first_touch_timestamp,
  email,
  updated,
  first_touch,
  to_date(date_format(first_touch, "yyy-M-d")) AS first_touch_date,
  date_format(first_touch, "HH:mm:ss") AS first_touch_time,
  regexp_extract(email, "@(.*)", 0) AS email_domain
FROM (
  SELECT user_id,
    user_first_touch_timestamp,
    CAST(user_first_touch_timestamp / 1e6 AS timestamp) AS first_touch,
    email,
    updated
  FROM users_silver_working
));
```

# Complex Transformations

## Data Overview

The **`events_raw`** table was registered against data representing a Kafka payload. In most cases, Kafka data will be binary-encoded JSON values. 

Let's cast the **`key`** and **`value`** as strings to view these values in a human-readable format.

```sql
CREATE OR REPLACE TEMP VIEW events_strings AS 
SELECT 
  CAST(key AS string) AS key,  string(value) 
FROM events_raw;



SELECT * 
FROM events_strings LIMIT 10;
```

## Work with Nested Data
Spark SQL has built-in functionality to directly interact with nested data stored as JSON strings or struct types.
- Use **`:`** syntax in queries to access subfields in JSON strings
- Use **`.`** syntax in queries to access subfields in struct types

Let's step into the **`value`** column and grab one row of data with an **`event_name`** of "finalize."

```sql
SELECT * 
FROM events_strings 
WHERE value:event_name = "finalize" 
ORDER BY key 
LIMIT 1;
```
Let's use the **JSON** string example above to derive the schema, then parse the entire **JSON** column into **STRUCT** types.
- **`schema_of_json()`** returns the schema derived from an example **JSON** string.
- **`from_json()`** parses a column containing a **JSON** string into a **STRUCT** type using the specified schema.

After we unpack the **JSON** string to a **STRUCT** type, let's unpack and flatten all **STRUCT** fields into columns.

**`*`** unpacking can be used to flatten a **STRUCT**; **`col_name.*`** pulls out the subfields of **`col_name`** into their own columns.

```sql
SELECT schema_of_json('{"device":"Linux","ecommerce":{"purchase_revenue_in_usd":1075.5,"total_item_quantity":1,"unique_items":1},"event_name":"finalize","event_previous_timestamp":1593879231210816,"event_timestamp":1593879335779563,"geo":{"city":"Houston","state":"TX"},"items":[{"coupon":"NEWBED10","item_id":"M_STAN_K","item_name":"Standard King Mattress","item_revenue_in_usd":1075.5,"price_in_usd":1195.0,"quantity":1}],"traffic_source":"email","user_first_touch_timestamp":1593454417513109,"user_id":"UA000000106116176"}') AS schema;
```
```sql
CREATE OR REPLACE TEMP VIEW parsed_events AS 
SELECT json.* 
FROM (
  SELECT from_json(value, 'STRUCT<device: STRING, ecommerce: STRUCT<purchase_revenue_in_usd: DOUBLE, total_item_quantity: BIGINT, unique_items: BIGINT>, event_name: STRING, event_previous_timestamp: BIGINT, event_timestamp: BIGINT, geo: STRUCT<city: STRING, state: STRING>, items: ARRAY<STRUCT<coupon: STRING, item_id: STRING, item_name: STRING, item_revenue_in_usd: DOUBLE, price_in_usd: DOUBLE, quantity: BIGINT>>, traffic_source: STRING, user_first_touch_timestamp: BIGINT, user_id: STRING>') AS json 
  FROM events_strings);


SELECT * 
FROM parsed_events;
```
## Manipulate Arrays

Spark SQL has a number of functions for manipulating array data, including the following:
- **`explode()`** separates the elements of an array into multiple rows; this creates a new row for each element.
- **`size()`** provides a count for the number of elements in an array for each row.

The code below explodes the **`items`** field (an array of structs) into multiple rows and shows events containing arrays with 3 or more items.

```sql
CREATE OR REPLACE TEMP VIEW exploded_events AS
SELECT  *, 
  explode(items) AS item
FROM parsed_events;

-- Find users with more than 2 items in their cart
SELECT * 
FROM exploded_events 
WHERE size(items) > 2;
```
```sql
-- Confirm data type for new exploded 'item' column
DESCRIBE exploded_events;
```
## Nesting Functions
We may want to see a list of all events associated with each **`user_id`** and we can collect all items that have been in a user's cart at any time for any event. Let's walk through how we can accomplish this.
### Step 1
We use **`collect_set()`** to gather ("collect") all unique values in a group, including arrays. We use it here to collect all unique **`item_id`**'s in our **`items`** array of structs.


# SQL UDFs

Databricks do not recommend using python UDF as it loose a lot of optimizations. Ex.: python interpretor loader, converted ...

SQL user-defined functions:
- Persist between execution environments (which can include notebooks, DBSQL queries, and jobs).
- Exist as objects in the metastore and are governed by the same Table ACLs as databases, tables, or views.
- To **create** a SQL UDF, you need **`USE CATALOG`** on the catalog, and **`USE SCHEMA`** and **`CREATE FUNCTION`** on the schema.
- To **use** a SQL UDF, you need **`USE CATALOG`** on the catalog, **`USE SCHEMA`** on the schema, and **`EXECUTE`** on the function.

We can use **`DESCRIBE FUNCTION`** to see where a function was registered and basic information about expected inputs and what is returned (and even more information with **`DESCRIBE FUNCTION EXTENDED`**).

We can also view Functions in the Catalog Explorer

```sql
CREATE OR REPLACE FUNCTION item_preference(name STRING, price INT)
RETURNS STRING
RETURN CASE 
  WHEN name = "Standard Queen Mattress" THEN "This is my default mattress"
  WHEN name = "Premium Queen Mattress" THEN "This is my favorite mattress"
  WHEN price > 100 THEN concat("I'd wait until the ", name, " is on sale for $", round(price * 0.8, 0))
  ELSE concat("I don't need a ", name)
END;


SELECT *, 
  item_preference(name, price) 
FROM item_lookup;
```

# Advanced Delta Lake Features - LAB07

## [Liquid Clustering](https://docs.databricks.com/en/delta/clustering.html#what-is-liquid-clustering-used-for)

Delta Lake liquid clustering replaces table partitioning and ZORDER to simplify data layout decisions and optimize query performance. Liquid clustering provides flexibility to redefine clustering keys without rewriting existing data, allowing data layout to evolve alongside analytic needs over time.

Databricks recommends using liquid clustering for all new Delta tables.

We enable liquid clustering on a table by using **`CLUSTER BY`**. Databricks recommends choosing clustering keys based on commonly used query filters. Clustering keys can be defined in any order. 

´´´sql
CREATE OR REPLACE TABLE events_liquid 
CLUSTER BY (user_id) AS 
SELECT * 
FROM events;
´´´

´´´sql
ALTER TABLE events
CLUSTER BY (user_id);
´´´
```sql
DESCRIBE events;
```
'# Clustering Information' => user_id

### Triggering Liquid Clustering
Liquid clustering is incremental, meaning that data is only rewritten as necessary to accommodate data that needs to be clustered. Data files with clustering keys that do not match data to be clustered are not rewritten.

For best performance, Databricks recommends scheduling regular **`OPTIMIZE`** jobs to cluster data. For tables experiencing many updates or inserts, Databricks recommends scheduling an **`OPTIMIZE`** job every one or two hours. Because liquid clustering is incremental, most **`OPTIMIZE`** jobs for clustered tables run quickly.

```sql
OPTIMIZE events;
```

## The Delta Log
Each change to a table results in a new entry being written to the Delta Lake transaction log. 

The command, `DESCRIBE HISTORY` allows us to see this log.

```sql
DESCRIBE HISTORY students;
```

## Deletion Vectors
Note that the log includes an **OPTIMIZE** operation, yet we never called **`OPTIMIZE`** on the **`students`** table. If you open the `operationParameters` for the **`OPTIMIZE`** operation, you will see that `auto: true`. This is because Deletion Vectors triggered auto-compaction. When we delete rows from a table, Deletion Vectors mark those rows for deletion but do not re-write the underlying Parquet files. This helps reduce the so-called small file problem, where a table is made up of a large number of small Parquet files. However, Deletion Vectors will trigger auto-compaction, and the underlying files are re-written.

## Delta Lake Time Travel

Delta Lake gives us the opportunity to query tables at any point in the transaction log. These time travel queries can be performed by specifying either the version number or the timestamp.

```sql
SELECT * 
FROM students VERSION AS OF 3;
```

```sql
SELECT * 
FROM students TIMESTAMP AS OF '2025-01-17T12:18:58.000+00:00'
```

### Rollback Version

```sql
RESTORE TABLE students TO VERSION AS OF 8;
```

## Predictive Optimization
Predictive Optimization is a feature that can be enabled that takes away the necessity for manually performing **`OPTIMIZE`** and **`VACUUM`**.

With predictive optimization enabled, Databricks automatically identifies tables that would benefit from maintenance operations and runs them for the user. Maintenance operations are only run as necessary, eliminating both unnecessary runs for maintenance operations and the burden associated with tracking and troubleshooting performance.

You must enable predictive optimization at the account level. The feature is inherited by all lower-level objects, but it can be enabled/disabled on those objects, as needed.

#### View if Predictive Optimization is Enabled:
To check whether Predictive Optimization is enabled on a catalog, schema or table: 
```
DESCRIBE (CATALOG | SCHEMA | TABLE) EXTENDED name
```
 

**View Catalog**

`DESCRIBE CATALOG EXTENDED dbacademy;`

![alt text](Data_Ingestion_with_Delta_Lake\image-8.png)

**View Table**

`DESCRIBE TABLE EXTENDED events;`

![Table PO Check](Data_Ingestion_with_Delta_Lake\po_enabled_table.png)

<br></br>

#### Enabling Predictive Optimization:
- To enable Predictive Optimization view the [Enable predictive optimization](https://docs.databricks.com/en/optimizations/predictive-optimization.html) documentation.
```
ALTER CATALOG [catalog_name] {ENABLE | DISABLE} PREDICTIVE OPTIMIZATION;
ALTER {SCHEMA | DATABASE} [schema_name] {ENABLE | DISABLE} PREDICTIVE OPTIMIZATION;
ALTER TABLE [table_name] {ENABLE | DISABLE} PREDICTIVE OPTIMIZATION;
```