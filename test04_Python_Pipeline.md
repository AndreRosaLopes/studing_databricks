Question 1 of 27
 data engineer has a Delta Live Tables (DLT) pipeline that uses a change data capture (CDC) data source. They need to write a quality enforcement rule that ensures that records containing the values INSERT or UPDATE in the operation column cannot contain a null value in the name column. The operation column can contain one of three values: INSERT, UPDATE, and DELETE. If the constraint is violated, then the entire transaction needs to fail.  

 

Which of the following constraints can the data engineer use to enforce this rule? Select one response.

 


CONSTRAINT valid_id_not_null EXPECT (valid_id IS NOT NULL or operation = "INSERT")ON VIOLATION FAIL UPDATE

CONSTRAINT valid_operation EXPECT (operation IS NOT NULL) ON VIOLATION DROP ROW

CONSTRAINT valid_name EXPECT (name IS NOT NULL or operation = "DELETE") ON VIOLATION FAIL UPDATE

CONSTRAINT valid_name EXPECT (name IS NOT NULL or operation = "DELETE") ON VIOLATION DROP ROW

CONSTRAINT valid_operation EXPECT (valid_operation IS NOT NULL and valid_operation = "INSERT") ON VIOLATION DROP ROW
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
CONSTRAINT valid_name EXPECT (name IS NOT NULL or operation = "DELETE") ON VIOLATION FAIL UPDATE
Question 2 of 27
A data engineer has created the following code block to create a streaming live table from orders_bronze.

 

Code block:

@dlt.table

def orders_bronze():

    return (

        spark.readStream

            .format("cloudFiles")

            .option("cloudFiles.format", "json")

            .option("cloudFiles.inferColumnTypes", True)

            _____

            .select(

                F.current_timestamp().alias("processing_time"), 

                F.input_file_name().alias("source_file"), 

                "*"

            )

    )

 

Which of the following lines of code correctly fills in the blank? Assume the variable path represents the string file path to the data source. Select one response.

 


.write(f"{path}")

.read(f"{path}")

.loadStream(f"{path}")

.writeStream(f"{path}")

.load(f"{path}")
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
.load(f"{path}")
Question 3 of 27
Which of the following data quality metrics are captured through row_epectations in a pipeline’s event log? Select all that apply.

 


Name
(disabled)

Failed records
(disabled)

Dataset
(disabled)

Update ID
(disabled)

Flow progress
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
Dataset
Name
Failed records
Question 4 of 27
A data engineer is running a Delta Live Tables (DLT) notebook. However, in the first command they notice the following error:

 

Error:

 

ModuleNotFoundError: No module named 'dlt'

 

Which of the following statements explains this message? Select one response.

 


DLT notebooks must be run at scheduled intervals using the job scheduler.

DLT does not support the execution of Python and SQL notebooks within a single pipeline.

DLT queries must be connected to a pipeline using the pipeline scheduler.

DLT is not intended for interactive execution in a notebook.

DLT does not support the execution of Python notebooks.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
DLT is not intended for interactive execution in a notebook.
Question 5 of 27
Which of the following describes the advantages of the bronze layer of the multi-hop, medallion data architecture? Select one response.


The bronze layer reports data and uses de-normalized and read-optimized data models with a minimal number of joins.

The bronze layer brings data from different sources into an enterprise view, enabling self-service analytics for advanced analytics.

The bronze layer provides an historical archive of data lineage and auditability without rereading the data from the source system.

The bronze layer applies business rules and complex transformations for write-performant data models.

None of these responses correctly describe the advantages of the bronze layer in this data architecture.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The bronze layer provides an historical archive of data lineage and auditability without rereading the data from the source system.
Question 6 of 27
A data engineer wants to retrieve metrics on the latest update made to their pipeline. 

 

Which of the following approaches allows the data engineer to accomplish this? Select one response.

 


The data engineer can retrieve the update ID from the audit log where the event type is create_update.

The data engineer can retrieve the update ID from the audit log where the action type is user_action.

The data engineer can retrieve the update ID from the events log where the event type is last_update

The data engineer can retrieve the update ID from the events log where the action type is user_action.

The data engineer can retrieve the update ID from the events log where the event type is create_update.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The data engineer can retrieve the update ID from the events log where the event type is create_update.
Question 7 of 27
Which of the following correctly describes how Auto Loader ingests data? Select one response.


Auto Loader incrementally ingests new data files in batches.

Auto Loader writes new data files in incremental batches.

Auto Loader automatically detects new data files during manual or scheduled updates.

Auto Loader only detects new data files during scheduled update intervals.

Auto Loader automatically writes new data files continuously as they land.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Auto Loader incrementally ingests new data files in batches.
Question 8 of 27
A data engineer wants to retrieve metrics on the latest update made to their pipeline. The pipeline has multiple data sources. Despite the input data sources having low data retention, the data engineer needs to retain the results of their code indefinitely.

 

Which of the following statements identifies the type of table that needs to be used and why? Select one response.

 


Streaming live table; streaming live tables are always "correct", meaning their contents will match their definition after any update.

Streaming live table; streaming live tables record live metrics on the code.

Streaming live table; streaming live tables can preserve data indefinitely.

Live table; live tables only support reading from "append-only" streaming sources.

Live table; live tables retain the results of a command for up to 30 days.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Streaming live table; streaming live tables can preserve data indefinitely.
Question 9 of 27
A data engineer needs to review the events related to their pipeline and the pipeline’s configurations.

Which of the following approaches can the data engineer take to accomplish this? Select one response.

 


The data engineer can retrieve events of type user_action from the configured storage location.

The data engineer can select events of type user_action in the output table of the pipeline.

The data engineer can retrieve events of type user_action from the event log.

The data engineer can retrieve events of type user_action from the checkpoint directory.

The data engineer can select events of type user_action in the resultant DAG.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The data engineer can retrieve events of type user_action from the event log.
Question 10 of 27
A data engineer needs to examine how data is flowing through tables within their pipeline.

 

Which of the following correctly describes how they can accomplish this? Select one response.

 


The data engineer can retrieve the flow definition for the direct successor of the table and then combine the results.

The data engineer can retrieve the flow definition for the direct predecessor of each table and then combine the results.

The data engineer can combine the flow definitions for all of the tables into one command.

The data engineer can retrieve the flow definition for each table and then combine the results.

The data engineer can view the flow definition of each table in the pipeline from the Pipeline Events log.
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
The data engineer can retrieve the flow definition for the direct predecessor of each table and then combine the results.
Question 11 of 27
Which of the following correctly describes how to access contents of the table directory? Select one response.


The contents of the table directory can be viewed through the event log.

The contents of the table directory can be viewed through the checkpointing directory.

The contents of the table directory can be viewed through the flow definition’s output dataset.

The contents of the table directory can be viewed through the Auto Loader directory.

The contents of the table directory can be viewed through the metastore.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The contents of the table directory can be viewed through the metastore.
Question 12 of 27
Which of the following correctly describes how code from one library notebook can be referenced by code from another library notebook? Select one response.

 


Within a DLT Pipeline, code in any notebook library can reference tables and views created in any other notebook library.

Within a DLT Pipeline, code in notebook libraries cannot reference tables and views created in a different notebook library.

Within a DLT Pipeline, code in a notebook library can reference tables and views created in another notebook library as long as the referenced notebook library is installed on the other notebook library’s cluster.

Within a DLT Pipeline, code in a notebook library can reference tables and views created in another notebook library as long as one notebook library references the other notebook library.

Within a DLT Pipeline, code in a notebook library can reference tables and views created in another notebook library that is running on the same cluster.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Within a DLT Pipeline, code in any notebook library can reference tables and views created in any other notebook library.
Question 13 of 27
A data engineer has a Delta Live Tables (DLT) pipeline that uses a change data capture (CDC) data source. They need to write a quality enforcement rule that ensures that values in the column operation do not contain null values. If the constraint is violated, the associated records cannot be included in the dataset. 

 

Which of the following constraints does the data engineer need to use to enforce this rule? Select one response.

 


CONSTRAINT valid_operation ON VIOLATION FAIL UPDATE

CONSTRAINT valid_operation EXPECT (operation IS NOT NULL)

CONSTRAINT valid_operation EXCEPT (operation) ON VIOLATION DROP ROW

CONSTRAINT valid_operation EXCEPT (operation != null) ON VIOLATION FAIL UPDATE

CONSTRAINT valid_operation EXPECT (operation IS NOT NULL) ON VIOLATION DROP ROW
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
CONSTRAINT valid_operation EXPECT (operation IS NOT NULL) ON VIOLATION DROP ROW
Question 14 of 27
A data engineer has created the following code block to create the view subscribed_order_emails from orders_silver and customers_silver.

 

Code block:

@dlt.view

def subscribed_order_emails_v():

    return (

        dlt.read("orders_silver").filter("notifications = 'Y'").alias("a")

            _____

            ).select(

                "a.customer_id", 

                "a.order_id", 

                "b.email"

            )

    )

 

 

 


.join(
dlt.readStream("customers_silver").alias("b"),
on="customer_id"

.join(
dlt.read("customers_silver").alias("a"),
on="customer_id"

.join(
dlt.read("customers_silver").alias("b"),
on="customer_id"

.join(
dlt.readStream("customers_silver").alias("a"),
on="customer_id"

.join(
dlt.merge("customers_silver").alias("b"),
on="customer_id"
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
.join(
dlt.read("customers_silver").alias("b"),
on="customer_id"
Question 15 of 27
A data engineer is creating a live streaming table to be used by other members of their team. They want to indicate that the table contains silver quality data.

 

Which of the following describes how the data engineer can clarify this to other members of their team? Select two responses.

 


comment = "This is a silver table."
(disabled)

@dlt.expect_or_fail("quality" == "silver")
(disabled)

table_properties = {"quality": "silver"}
(disabled)

expectation = {"quality": "silver"}
(disabled)

None of these answer choices are correct.
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
table_properties = {"quality": "silver"}
comment = "This is a silver table."
Question 16 of 27
A data engineer needs to return the result set for a Delta Live Table (DLT) in a notebook. The notebook is not attached to a DLT pipeline.

  

Which of the following correctly describes the form of results that the code returns? Select one response.

 


Commands outside of DLT will return snapshot results from DLT tables only if they were defined as a batch table.

Commands outside of DLT will return the most recent version from DLT tables only if they were defined as a streaming table.

Commands outside of DLT will return snapshot results from DLT tables, regardless of how they were defined.

Commands outside of DLT will return the most recent version from DLT tables, regardless of how they were defined.

Commands outside of DLT will return snapshot results from DLT tables only if they were defined as a streaming table.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Commands outside of DLT will return snapshot results from DLT tables, regardless of how they were defined.
Question 17 of 27
A data engineer has built and deployed a DLT pipeline. They want to see the output for each individual task.

 

Which of the following describes how to explore the output for each task in the pipeline? Select one response.

 


They can go to the Pipeline Details page and click on the individual tables in the resultant Directed Acyclic Graph (DAG).

They can specify a folder for the task run details during pipeline configuration.

They can go to the Job Runs page and click on the individual tables in the job run history.

They can display the output for each individual command from within the notebook using the %run command.

They can run the commands connected to each task from within the DLT notebook after deploying the pipeline.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
They can go to the Pipeline Details page and click on the individual tables in the resultant Directed Acyclic Graph (DAG).
Question 18 of 27
A data engineer has built and deployed a DLT pipeline. They want to perform an update that writes a batch of data to the output directory. 

 

Which of the following statements about performing this update is true? Select one response.

 


All newly arriving data will be continuously processed through their pipeline. Metrics will be reported for the current run if specified during pipeline deployment

With each triggered update, all newly arriving data will be processed through their pipeline. Metrics will be reported if specified during pipeline deployment.

All newly arriving data will be continuously processed through their pipeline. Metrics will always be reported for the current run

With each triggered update, all newly arriving data will be processed through their pipeline. Metrics will not be reported for the current run.

With each triggered update, all newly arriving data will be processed through their pipeline. Metrics will always be reported for the current run.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
With each triggered update, all newly arriving data will be processed through their pipeline. Metrics will always be reported for the current run.
Question 19 of 27
A data engineer needs to add a file path to their DLT pipeline. They want to use the file path throughout the pipeline as a parameter for various statements and functions.

 

Which of the following options can be specified during the configuration of a DLT pipeline in order to allow this? Select one response.

 


They can add a widget to the notebook and then perform a string substitution of the file path.

They can specify the file path in the job scheduler when deploying the pipeline.

They can add a parameter when scheduling the pipeline job and then perform a variable substitution of the file path.

They can add a key-value pair in the Configurations field and then perform a string substitution of the file path.

They can set the variable in a notebook command and then perform a variable substitution of the file path.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
They can add a key-value pair in the Configurations field and then perform a string substitution of the file path.
Question 20 of 27
Which of the following statements provide examples of contrasting use cases for silver and gold tables? Select one response.


Silver tables provide business level aggregates often used for reporting and dashboarding. Gold tables reduce data storage complexity, latency, and redundancy.

Silver tables contain raw data ingested from various sources. Gold tables provide efficient storage and querying of unprocessed data.

Silver tables contain filtered or cleansed data. Gold tables contain raw data ingested from various sources.

Silver tables enrich data by joining fields from bronze tables. Gold tables provide business level aggregates often used for reporting and dashboarding.

Silver tables retain the full, unprocessed history of each data set. Gold tables provide business level aggregates often used for reporting and dashboarding.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Silver tables enrich data by joining fields from bronze tables. Gold tables provide business level aggregates often used for reporting and dashboarding.
Question 21 of 27
Which of the following are advantages of using a Delta Live Tables (DLT) pipeline over a traditional ETL pipeline in Databricks? Select two responses.


DLT automates data management through physical data optimizations and schema evolution.
(disabled)

DLT has built-in quality controls and data quality monitoring.
(disabled)

DLT decouples compute and storage costs regardless of scale.
(disabled)

DLT provides granular observability into pipeline operations and automatic error handling.
(disabled)

DLT leverages additional metadata over other open source formats such as JSON, CSV, and Parquet.
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
DLT has built-in quality controls and data quality monitoring.
DLT provides granular observability into pipeline operations and automatic error handling.
Question 22 of 27
A data engineer is using the following code block to create a live table. After running the code block, they notice an error. 

 

Code block:

@dlt.view

def subscribed_order_emails():

    return (

        dlt.read("orders_silver").filter("notifications = 'Y'").alias("a")

            .join(

                dlt.read("status_silver").alias("b"), 

                on="status_id"

            ).select(

                "a.status_id", 

                "a.order_id", 

                "b.email"

            )

    )




Which of the following statements correctly identifies the error? Select one response.

 


The .read() function needs to be changed to .readStream().

"left" needs to be added to the .join() function.

None of these statements correctly identifies the error.

"a.status_id" needs to be changed to "b.status_id" in the .select() function.

The .join() function needs to be changed to .merge().
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The .read() function needs to be changed to .readStream().
Question 23 of 27
Which of the following statements accurately describes the difference in behavior between live views and live tables? Select one response.


The results of live tables can be viewed through a Directed Acyclic Graph (DAG), while the results for live views cannot.

Metrics for live tables can be collected and reported, while data quality metrics for views are abstracted to the user.

The results of live tables are stored to disk, while the results of views can only be referenced from within the DLT pipeline in which they are defined.

Live tables can be used to enforce data quality, while views do not have the same guarantees in schema enforcement.

Live tables can be used with a stream as its source, while live views are incompatible with structured streaming.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The results of live tables are stored to disk, while the results of views can only be referenced from within the DLT pipeline in which they are defined.
Question 24 of 27
A data engineer needs to identify the cloud provider and region of origin for each event within their DLT pipeline.

 

Which of the following approaches allows the data engineer to view this information? Select one response. 

 


The data engineer can load the contents of the event log into a view and display the view.

The data engineer can view the event details for the pipeline from the resultant Directed Acyclic Graph (DAG).

The data engineer can view this information in the Task Details page for each task in the pipeline.

The data engineer can use the .select() function to directly retrieve the cloud_details field of the event.

The data engineer can use a utility command in Python to list information about each update made to a particular data object.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The data engineer can load the contents of the event log into a view and display the view.
Question 25 of 27
A data engineer is configuring a new DLT pipeline and is unsure of what mode to choose. They are working with a small batch of unchanging data and need to minimize the costs associated with the pipeline.

 

Which of the following modes do they need to use and why? Select one response.  

 


Triggered; triggered pipelines run once and then shut down until the next manual or scheduled update.

Continuous; continuous pipelines run at set intervals and then shut down until the next manual or scheduled update.

Triggered; triggered pipelines update once and cannot be updated again for 24 hours.

Continuous; continuous pipelines ingest new data as it arrives.

Triggered; triggered pipelines update once and cannot be updated again until they are manually run.
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
Triggered; triggered pipelines run once and then shut down until the next manual or scheduled update.
Question 26 of 27
A data engineer has configured and deployed a DLT pipeline that contains an error. The error is thrown at the third stage of the pipeline, but since DLT resolves the order of tables in the pipeline at different steps, they are not sure if the first stage succeeded.

 

Which of the following is considered a good practice to determine this? Select one response.

 


The data engineer can fix the tables from the Directed Acyclic Graph (DAG), starting at their earliest dataset.

The data engineer can fix the tables using iterative logic, starting at their earliest dataset.

The data engineer can fix one table at a time, starting at their earliest dataset.

The data engineer can fix the tables from the Directed Acyclic Graph (DAG), starting at the dataset containing the error.

The data engineer can fix the tables using iterative logic, starting at the dataset containing the error.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The data engineer can fix one table at a time, starting at their earliest dataset.
Question 27 of 27
Which of the following are guaranteed when processing a CDC feed with dlt.apply_changes()? Select all that apply.

 


dlt.apply_changes() assumes by default that rows will contain inserts and updates.
(disabled)

dlt.apply_changes() automatically quarantines late-arriving data in a separate table.
(disabled)

dlt.apply_changes() defaults to creating a Type 1 SCD table.
(disabled)

dlt.apply_changes() supports insert-only and append-only data.
(disabled)

dlt.apply_changes() automatically orders late-arriving records using a user-provided sequencing key.
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
dlt.apply_changes() defaults to creating a Type 1 SCD table.
dlt.apply_changes() automatically orders late-arriving records using a user-provided sequencing key.
dlt.apply_changes() assumes by default that rows will contain inserts and updates.