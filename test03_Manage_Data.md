Question 1 of 26
A data engineer is using the code shown below to replace data from the table sales with data from a new query. However, the query isn’t running as expected. 

 

Code block:

 

INSERT INTO sales

SELECT *, current_timestamp() FROM parquet `${da.paths.datasets}/ecommerce/raw/sales-historical`




Which of the following statements correctly explains why the query is not running as expected? Select one response.

 


The source file path is formatted incorrectly. Double-quotes need to be used in place of back-ticks.

APPEND needs to be used instead of INSERT INTO.

None of the provided answer choices explain why the query is running incorrectly.

INSERT OVERWRITE needs to be used instead of INSERT INTO.

MERGE INTO needs to be used instead of INSERT INTO.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
INSERT OVERWRITE needs to be used instead of INSERT INTO.
Question 2 of 26
Which of the following validates that the temporary view trees has exactly six records? Select one response.


assert spark.load("trees").count() == 6

assert spark.table("trees").count() == 6

assert spark("trees").count() == 6

assert spark.view("trees").count() == 6

assert spark.temp("trees").count() == 6
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
assert spark.table("trees").count() == 6
Question 3 of 26
A data engineer wants to review the changes that other team members have made to the table cities, including the operations that were performed on the table and the time at which they were performed. The variable path represents the file path to the table.

 

Which of the following commands does the data engineer need to use? Select one response.

 


SELECT * FROM cities VERSION AS OF 1;

display(dbutils.fs.ls(f"{path}"))

DESCRIBE EXTENDED cities;

DESCRIBE HISTORY cities;

DESCRIBE DETAIL cities;
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
DESCRIBE HISTORY cities;
Question 4 of 26
A data engineer wants to create an empty Delta table called student if it hasn’t already been created.


Which of the following will create a new table named student regardless of whether another table with the same name has already been created? Select one response.


CREATE OR REPLACE TABLE student (id INT, name STRING, age INT);

OVERWRITE TABLE student (id INT, name STRING, age INT);

CREATE TABLE IF NOT EXISTS student AS SELECT * FROM student

CREATE TABLE student (id INT, name STRING, age INT);

CREATE TABLE IF NOT EXISTS student (id INT, name STRING, age INT);
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
CREATE OR REPLACE TABLE student (id INT, name STRING, age INT);
Question 5 of 26
A data engineer is trying to optimize the result set returned in their query by compacting their data files to avoid small files. 

 

Which keyword do they need to use in their query to improve the query’s performance while keeping the data files as even as possible with respect to size on disk? Select one response.


COMPACT

VACUUM

ZORDER

OPTIMIZE

DATASKIPPING
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
OPTIMIZE
Question 6 of 26
A data engineer needs to atomically append new rows to an existing Delta table. 

 

Which of the following approaches is considered best practice to efficiently modify the table? Select one response.

 


The data engineer can use INSERT OVERWRITE to incrementally update the existing tables.

The data engineer can use INSERT ONLY to incrementally update the existing tables.

The data engineer can use INSERT INTO to incrementally update the existing tables.

The data engineer can use UPDATE to update the existing tables in one batch.

The data engineer can use APPEND to update the existing tables in one batch.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The data engineer can use INSERT INTO to incrementally update the existing tables.
Question 7 of 26
A data engineer has a collection of tables. They need to manually remove old data files from the tables and remove access to previous versions of the tables. 

 

Which of the following approaches allows the data engineer to do this? Select one response.

 


They need to enable the retention duration check and vacuum logging. Then they need to optimize the table.

They need to disable the retention duration check and enable vacuum logging. Then they need to vacuum the table.

They need to disable the last commit version in session and enable vacuum duration check. Then they need to Z-order the table.

They need to disable the retention duration check and enable the last commit version in session. Then they need to vacuum the table.

They need to enable the retention duration check and disable vacuum logging. Then they need to Z-order the table.
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
They need to disable the retention duration check and enable vacuum logging. Then they need to vacuum the table.
Question 8 of 26
Which of the following pieces of information about a table are located within the schema directory of the table? Select three responses.


Location
(disabled)

Last modification date
(disabled)

Catalog name
(disabled)

Creation date
(disabled)

Owner
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
Catalog name
Location
Owner
Question 9 of 26
Which of the following conditions must be met for data to be loaded incrementally with the COPY INTO command? Select three responses.

 


The data must be in JSON or CSV format.
(disabled)

The source file must specify the file’s format.
(disabled)

The schema for the data must be defined.
(disabled)

The data cannot contain duplicate records.
(disabled)

COPY INTO must target an existing Delta table.
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
The source file must specify the file’s format.
COPY INTO must target an existing Delta table.
The data must be in JSON or CSV format.
Question 10 of 26
A data engineer is working with the table products. They want to identify the location of products and read its metadata, including the table’s format and the date that the table was created at.

 

Which of the following commands do they need to use? Select one response.

 


DESCRIBE HISTORY products;

DESCRIBE TABLE EXTENDED products;

DESCRIBE TABLE products;

SHOW TABLES products;

DESCRIBE DETAIL products;
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
DESCRIBE DETAIL products;
Question 11 of 26
A data engineer is trying to create the generated column date in their table. However, when they run their query to create the table, they notice an error in the following line of code.

 

Line of code:

date DATE GENERATED ALWAYS AS (

    cast(cast(transaction_timestamp/1e6) AS DATE)))

 

Which of the following statements correctly describes the error? Select one response.

 


transaction_timestamp needs to be cast as a timestamp before it is cast as a date.

The ALWAYS keyword needs to be removed to account for improperly formatted data.

transaction_timestamp needs to be converted to datetime format before it is cast as a date.

transaction_timestamp needs to be converted to an integer before it is cast as a date.

The DATE GENERATED ALWAYS AS command already casts transaction_timestamp to a date, so the AS DATE cast needs to be removed.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
transaction_timestamp needs to be cast as a timestamp before it is cast as a date.
Question 12 of 26
 data engineer wants to make changes to a very large table. They want to test their changes on a similar data object before modifying or copying the original table’s associated data.

 

Which of the following keywords can be used to create a similar data object that can be used for testing while meeting the above requirements? Select one response.

 


COPY

UPDATE

SHALLOW CLONE

CLONE

DEEP CLONE
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
SHALLOW CLONE
Question 13 of 26
A data engineer needs to update the table people. The engineer only wants records to be updated if the existing row has a NULL value in the column email and the new row does not.

 

They have the following incomplete code block:

 

MERGE INTO people a

USING people_update b

ON a.people_id = b.people_id

_____

WHEN NOT MATCHED THEN DELETE

 

Which of the following statements correctly fills in the blank? Select one response.

 


INSERT (email = b.email) WHEN MATCHED AND a.email IS NULL

WHEN MATCHED AND a.email IS NULL THEN INSERT (email = b.email)

UPDATE SET email = b.email WHEN a.email IS NULL

UPDATE SET email = b.email WHEN MATCHED AND a.email IS NULL

WHEN MATCHED AND a.email IS NULL THEN UPDATE SET email = b.email
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
WHEN MATCHED AND a.email IS NULL THEN UPDATE SET email = b.email
Question 14 of 26
A data engineer needs to create the new database clients at a location represented by the variable path. The database will only contain JSON files.

 

Which of the following commands does the data engineer need to run to complete this task? Select two responses.

 


CREATE SCHEMA IF NOT EXISTS clients json. '${path}';
(disabled)

CREATE DATABASE IF NOT EXISTS clients '${path}';
(disabled)

CREATE DATABASE clients LOCATION '${path}';
(disabled)

CREATE DATABASE clients DELTA json. '${path}';
(disabled)

CREATE SCHEMA clients LOCATION '${path}';
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
CREATE SCHEMA clients LOCATION '${path}';
CREATE DATABASE clients LOCATION '${path}';
Question 15 of 26
Which of the following problems are solved by the guarantee of ACID transactions? Select two responses.


ACID transactions support the creation of interactive visualization queries.
(disabled)

ACID transactions guarantee that appends will not fail due to conflict, even when writing from multiple sources at the same time.
(disabled)

ACID transactions guarantee the use of proprietary storage formats.
(disabled)

ACID transactions combine compute and storage scaling to reduce costs.
(disabled)

ACID transactions are guaranteed to either succeed or fail completely, so jobs will never fail mid way.
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
ACID transactions are guaranteed to either succeed or fail completely, so jobs will never fail mid way.
ACID transactions guarantee that appends will not fail due to conflict, even when writing from multiple sources at the same time.
Question 16 of 26
A data engineer needs to undo changes made to the table foods. They need to ensure that the second version of the table does not include the changes before restoring the table back to that state.




Lines of code:

 

SELECT * FROM foods VERSION AS OF 2;

REFRESH TABLE foods;

SELECT * FROM foods WHERE version_number() == 2;

RESTORE TABLE foods TO VERSION AS OF 2;

 

In what order do the lines of code above need to be run in a SQL environment in order to meet the requirements? Select one response.

 


1,4

2

4

1,2

3,2
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
1,4
Question 17 of 26
Which of the following statements about vacuuming with Delta Lake is true? Select one response.

 


Delta table metadata files will not be vacuumed unless the auto retention check is turned off.

VACUUM will not vacuum any directories that begin with an underscore except for _delta_log.

Delta table data files are vacuumed according to their modification timestamps on the storage system.

On Delta tables, Databricks automatically triggers VACUUM operations as data is written.

Running VACUUM on a Delta table eliminates the ability to time travel back to a version older than the specified data retention period
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Running VACUUM on a Delta table eliminates the ability to time travel back to a version older than the specified data retention period
Question 18 of 26
The code block shown below should add a constraint to the table transaction_dates where only records from after '2022-10-01' can be added to the table. The column date represents the date the records were created.

 

Code block: 

__1__ transaction_dates __2__ valid_date __3__;

 

Which of the following correctly fills in the numbered blanks within the code block to complete this task? Select one response.


1. ALTER TABLE

2. DROP CONSTRAINT

3. NOT NULL (date > '2022-10-01')


1. ALTER TABLE

2. CONSTRAINT

3. (date > '2022-10-01')


1. ALTER TABLE

2. ADD CONSTRAINT

3. WHERE (date > '2022-10-01')


1. ALTER TABLE

2. ADD CONSTRAINT

3. CHECK (date > '2022-10-01')


1. ALTER TABLE

2. CONSTRAINT

3. UPDATE WHEN (date > '2022-10-01')

Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
1. ALTER TABLE

2. ADD CONSTRAINT

3. CHECK (date > '2022-10-01')

Question 19 of 26
Which of the following SQL commands can be used to remove a schema (database) at a specified location? Select two responses.

 


DELETE DATABASE
(disabled)

DROP SCHEMA
(disabled)

DROP DATABASE
(disabled)

REMOVE DATABASE
(disabled)

DELETE SCHEMA
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
DROP DATABASE
DROP SCHEMA
Question 20 of 26
A data engineer needs to query a Delta table to extract rows that all meet the same condition. However, they notice that the query is running slowly, and that the data files used in the query are extremely small.

 

Which of the following techniques can the data engineer use to improve the performance of the query? Select one response.


They can perform vacuuming and data skipping in the query using the VACUUM and DATASKIPPING commands.

They can perform file compaction and Z-order indexing in the query using the OPTIMIZE and ZORDER commands.

They can perform file compaction and vacuuming in the query using the COMPACT and VACUUM commands.

They can perform data skipping and file compaction in the query using the DATASKIPPING and OPTIMIZE commands

They can perform file compaction and Z-order indexing in the query using the COMPACT and ZORDER commands.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
They can perform file compaction and Z-order indexing in the query using the OPTIMIZE and ZORDER commands.
Question 21 of 26
Which of the following describes a feature of Delta Lake that is unavailable in a traditional data warehouse? Select two responses.


Centralized repository to share features
(disabled)

Built-in monitoring for scheduled queries
(disabled)

Experiment tracking and model management
(disabled)

Combined batch and streaming analytics
(disabled)

Auto Loader for data ingestion of raw files
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
Combined batch and streaming analytics
Auto Loader for data ingestion of raw files
Question 22 of 26
Which of the following statements about managed and external tables are true? Select two responses.

 


When dropping a managed table, only the underlying metadata stays intact.
(disabled)

External tables will always specify a LOCATION during table creation.
(disabled)

When moving a managed table to a new database, the table’s data must be written to the new location.
(disabled)

Managed tables are specified with the CREATE MANAGED TABLE command in SQL.
(disabled)

When dropping an external table, the underlying data and metadata are also deleted.
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
When moving a managed table to a new database, the table’s data must be written to the new location.
External tables will always specify a LOCATION during table creation.
Question 23 of 26
A data engineer is trying to improve the performance of their query by colocating records on a common filter column to reduce the number of files that need to be read. The data engineer notices that the column user_id, which contains only unique values, is used in all of their query predicates. 

 

Which optimization technique does the data engineer need to use? Select one response.

 


The data engineer needs to use DATASKIPPING to colocate records on user_id.

The data engineer needs to use COLOCATE to colocate records on user_id.

The data engineer needs to use VACUUM to colocate records on user_id.

The data engineer needs to use ZORDER to colocate records on user_id.

The data engineer needs to use OPTIMIZE to colocate records on user_id.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The data engineer needs to use ZORDER to colocate records on user_id.
Question 24 of 26
Which of the following table modifications can be made with a MERGE INTO statement? Select three responses.

 


Write a stream of schema changes into a table
(disabled)

Write data that generates multiple downstream tables
(disabled)

Write raw data from a file location into a schema
(disabled)

Write data to a table with automatic deduplication
(disabled)

Write streaming aggregates in Update Mode
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
Write data to a table with automatic deduplication
Write streaming aggregates in Update Mode
Write a stream of schema changes into a table
Question 25 of 26
An organization’s data warehouse team is using a change data capture (CDC) feed that needs to meet the CCPA compliance standards. They are worried that their current architecture will not support this workload.

 

Which of the following explains how employing Delta Lake in a data lakehouse architecture addresses these concerns? Select one response.

 


Delta Lake supports integration for experiment tracking and built-in ML best practices.

Delta Lake supports automatic logging of experiments, parameters and results from notebooks directly to MLflow.

Delta Lake supports merge, update and delete operations to enable complex use cases.

Delta Lake supports expectations to define expected data quality and specify how to handle records that fail those expectations.

Delta Lake supports data management for transformations based on a target schema for each processing step.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Delta Lake supports merge, update and delete operations to enable complex use cases.
Question 26 of 26
A data engineer needs to create a table with additional metadata columns.  The columns need to specify the timestamp at which the table creation query was executed and the source data file for each record in the table. 

 

Which of the following built-in Spark SQL commands can the data engineer use in their query to add these columns? Select two responses.

 


input_file_name()
(disabled)

from_utc_timestamp()
(disabled)

input_file_block_start()
(disabled)

current_timestamp()
(disabled)

from_unixtime()
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
current_timestamp()
input_file_name()