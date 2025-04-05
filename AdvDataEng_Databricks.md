# Incremental Prossing with Spark Structured Streaming
What is streaming data? Continuously generated and unbounded data.
![alt text](image-20.png)

![alt text](image-21.png)

Use cases:
* Notifications
* Real-time reporting
* Incremental ETL
* Updata data to serve in real-time
* Real-time decision making (bank: credit card frauld...)
* Online ML

![alt text](image-22.png)

![alt text](image-23.png)

![alt text](image-24.png)

![alt text](image-25.png)

![alt text](image-26.png)

![alt text](image-27.png)

![alt text](image-28.png)

# Structured Streaming
engine: Spark Structured Streaming

Spark Structured Streaming is a scalable, fault-tolerant stream processing framework built on Spark SQL engine. It uses existing structured APIs (DataFrames, SQL Engine) and provides similar API as batch processing API.

Features

End-to-end
Exactly-once processing
Fault-tolerance

![alt text](image-29.png)

* Micro-batch execution: acuumulate small batches of data and process each batch in **parallel**.
* Continuous execution (experimental): continuously listen for new data and process them **individually**. Overhead per record. Load balance issues.

![alt text](image-30.png)

![alt text](image-31.png)

![alt text](image-32.png)

![alt text](image-33.png)

![alt text](image-34.png)

![alt text](image-35.png)

![alt text](image-36.png)

![alt text](image-37.png)

![alt text](image-38.png)

## Benefits
![alt text](image-39.png)

![alt text](image-40.png)

![alt text](image-41.png)

![alt text](image-42.png)

![alt text](image-43.png)

![alt text](image-44.png)

## Lab 1.1 Reading from a Streaming Query

##### Classes
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.ss/api/pyspark.sql.streaming.DataStreamReader.html" target="_blank">DataStreamReader</a>
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.ss/api/pyspark.sql.streaming.DataStreamWriter.html" target="_blank">DataStreamWriter</a>
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.ss/api/pyspark.sql.streaming.StreamingQuery.html" target="_blank">StreamingQuery</a>

## Aggregations, Time Windows, Watermarks

![alt text](image-45.png)

![alt text](image-46.png)

![alt text](image-47.png)

Watermark - is a feature in Struct Streaming to allow you specify how late you expect to see data in event time.
Data later than threshold will be dropped ( not considered in aggregation)

Time Domain Skew

![alt text](image-48.png)

![alt text](image-49.png)

## Delta Live Tables

![alt text](image-50.png)

![alt text](image-51.png)

![alt text](image-52.png)

With DLT, engineers are able to treat their data as code and apply modern software engineering best practices like testing, error handling, monitoring, and documentation to deploy reliable pipelines at scale.

![alt text](image-53.png)

![alt text](image-54.png)

![alt text](image-55.png)

# Streaming ETL Patterns with DLT

![alt text](image-56.png)

## Pattern #1

![alt text](image-57.png)

TBLPROPERTIES(pipelines.reset.allowed=false) => If we delete raw data, the bronze table would not be recomputed.

The general best practice is to have a streaming table for bronze data and live tables for silver and gold, especially for pipelines that involve sensitive data.

## Pattern #2
![alt text](image-58.png)


## Pattern #3
![alt text](image-59.png)

![alt text](image-60.png)

The problem of simplex:
* Parsing to multiple tables requires multiple streams, as well as a separate process to write each silver table.
* Skew in data volume and velocity can significantly impact throughput.
When a stream gets too backed up, batch sizes can balloon and batch latencies can introduce significant delays over time. If a stream is not monitored properly, data may be purged from kafka before it can be ingested.

Essentially, multiplex ingestion is a low cost buffer and built in safety net that allows for all streaming data to be processed more efficiently.

Quando um fluxo fica muito congestionado, o tamanho dos lotes pode aumentar e as latências dos lotes podem introduzir atrasos significativos ao longo do tempo.

![alt text](image-61.png)

<img src="image-61.png" width="40%" />

<img src="https://files.training.databricks.com/images/ade/ADE_arch_bronze.png" width="60%" />

## Data Quality Enforcement Patterns
## Data Modeling
## Streaming joins and Startefulness

<img src="image-62.png" width="60%" />

<img src="image-63.png" width="60%" />

![alt text](image-64.png)

![alt text](image-65.png)

### Backfills

![alt text](image-66.png)

# Data Privacy and Governance Patterns

## Store Data Securely

PII - Personal Identifiable Information

![alt text](image-67.png)

![alt text](image-68.png)

![alt text](image-69.png)

![alt text](image-70.png)

![alt text](image-71.png)

![alt text](image-72.png)

![alt text](image-73.png)

![alt text](image-74.png)

![alt text](image-75.png)

![alt text](image-76.png)

![alt text](image-77.png)

![alt text](image-78.png)

![alt text](image-79.png)

![alt text](image-80.png)

## Streaming Data and Change Data Feed

Apply changes into - magic world!

![alt text](image-81.png)

![alt text](image-82.png)

![alt text](image-83.png)

![alt text](image-84.png)

Regarding materialized views, there's a need to capture an aggregated view of the gold level data for a dashboard or real time application. No need for re-aggregations off of full tables, while ensuring that changes are reflected appropriately.

![alt text](image-85.png)

![alt text](image-86.png)

## Deleting Data in the Lakehouse

![alt text](image-87.png)

![alt text](image-88.png)

![alt text](image-89.png)

![alt text](image-90.png)

![alt text](image-91.png)

# Performance Optimization with Spark and Delta Lake

## Designing the Foundation

![alt text](image-92.png)

![alt text](image-93.png)

![alt text](image-94.png)

z-order first 32 cols.

![alt text](image-95.png)

![alt text](image-96.png)

![alt text](image-97.png)

![alt text](image-98.png)

## Code Optimization

![alt text](image-99.png)

![alt text](image-100.png)

## Assess and Debug Spark Applications

![alt text](image-101.png)

![alt text](image-102.png)

![alt text](image-103.png)

![alt text](image-104.png)

![alt text](image-105.png)

![alt text](image-106.png)

![alt text](image-107.png)

## Spark UI Simulator

![alt text](image-108.png)

## Shuffles

![alt text](image-109.png)

![alt text](image-110.png)

![alt text](image-111.png)

![alt text](image-112.png)

AQE - Adaptive Query Execution

CBO - cost-based optimizer

![alt text](image-114.png)

![alt text](image-113.png)

![alt text](image-115.png)

## Spill

![alt text](image-116.png)

![alt text](image-117.png)

![alt text](image-118.png)

![alt text](image-119.png)

## Skew

![alt text](image-120.png)

![alt text](image-121.png)

![alt text](image-122.png)

![alt text](image-123.png)

## Serialization

![alt text](image-124.png)

![alt text](image-125.png)

![alt text](image-126.png)

## Fine-Tuning: Choosing the Right

![alt text](image-127.png)

![alt text](image-128.png)

![alt text](image-129.png)

![alt text](image-130.png)

![alt text](image-131.png)

![alt text](image-132.png)

# 5. Softwares Engineer Practices for DLT Pipelines

CI/CD for DLT 

![alt text](image-133.png)

![alt text](image-134.png)

![alt text](image-135.png)

![alt text](image-136.png)

# 6. Automate Prouction Workflows

## Introduction to the REST API and CLI

![alt text](image-137.png)

![alt text](image-138.png)

![alt text](image-139.png)

![alt text](image-140.png)

![alt text](image-141.png)


## 1st step: Generate Tokens

### A - Verify that Personal Access Token are enabled in the workspace

1. Your Name >> Settings >> Developer >> Access tokens: Manage
![alt text](image-142.png)

2. Generate new token:
    Comment => put a identification or comment to specify the new token
    Lifetime (days) =>  between 1 and 730

![alt text](image-143.png)

3. Copy the token! >> Done
![alt text](image-144.png)

dapi79452b0bcabb0153cdfde0ecd66c6d39
dapi219d9346817c249f97e1845518535a11

4. If you wish, you can revoke your token:

![alt text](image-145.png)

## 2nd - Setup CLI in Cluster Web Terminal

1. View >> Web Terminal
![alt text](image-146.png)

1. Then, it open a terminal. Inside terminal type...
![alt text](image-147.png)

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
    ![alt text](image-148.png)

    for update a key, just do the same process ( ```databricks secrets put ...```)
    Recover the password1:
    ```
    password1 = dbutils.secrets.get(scope = "scope-name", key = "password1")
    ```
    list the scope:
    databricks secrets list-scopes

    list the keys:
    databricks secrets list --scopes projeto

![alt text](image-149.png)

    Remove scope:

    databricks secrets delete-scope --scope name

    Remove key:

    databricks secrets delete --scope name --key name
