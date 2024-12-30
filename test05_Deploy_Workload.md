Question 1 of 25
Which of the following components are necessary to create a Databricks Workflow? Select three responses.


Tasks
(disabled)

Experiment
(disabled)

Schedule
(disabled)

Cluster
(disabled)

Alert
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
Tasks
Schedule
Cluster
Question 2 of 25
A data engineering team has been using a Databricks SQL query to monitor the performance of an ELT job. The ELT job is triggered when a specific number of input records are ready to be processed. The Databricks SQL query returns the number of records added since the job’s most recent runtime. The team has manually reviewed some of the records and knows that at least one of them will be successfully processed without violating any constraints.

 

Which of the following approaches can the data engineering team use to be notified if the ELT job did not complete successfully? Select one response.

 


They can set up an alert for the job to notify them when a record has been added to the target dataset.

This type of alerting is not possible in Databricks.

They can set up an alert for the job to notify them when a record has been flagged as invalid.

They can set up an alert for the job to notify them if the returned value of the SQL query is less than 1.

They can set up an alert for the job to notify them when a constraint has been violated.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
They can set up an alert for the job to notify them if the returned value of the SQL query is less than 1.
Question 3 of 25
A lead data engineer needs the rest of their team to know when an update has been made to the status of a job run within a Databricks Job.  

 

How can the data engineer notify their team of the status of the job? Select one response.

 


They can schedule an email alert to themselves for when the job begins.

They can schedule a Dashboard alert to themselves for when the job succeeds.

They can schedule a Dashboard alert to a group containing all members of the team for when the job completes.

They can schedule an email alert to the whole team for when the job completes.

They can schedule a Dashboard alert to the whole team for when the job succeeds.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
They can schedule an email alert to the whole team for when the job completes.
Question 4 of 25
A data engineer needs to view the metadata concerning the order that events in a DLT pipeline were executed.

Which of the following steps can the data engineer complete to view this information? Select one response.

 


The data engineer can view the DLT metrics from the Pipeline Details page.

The data engineer can view the DLT metrics from the resultant Directed Acyclic Graph (DAG).

The data engineer can query the event log from a new notebook.

The data engineer can view the DLT metrics from the bar graph that is generated within the notebook.

The data engineer can query the metrics column of the event log for DLT metrics
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The data engineer can view the DLT metrics from the Pipeline Details page.
Question 5 of 25
Which of the following workloads can be configured using Databricks Workflows? Select three responses.


An ETL job with batch and streaming data
(disabled)

A job with Python, SQL, and Scala tasks
(disabled)

A data analysis job that uses R notebooks
(disabled)

A custom task where data is extracted from a JAR file
(disabled)

A job running on a triggered schedule with dependent tasks
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
A job running on a triggered schedule with dependent tasks
A custom task where data is extracted from a JAR file
An ETL job with batch and streaming data
Question 6 of 25
A data engineer is using Workflows to run a multi-hop (medallion) ETL workload. They notice that the workflow will not complete because one of the tasks is failing.

 

Which of the following describes the order of execution when running the repair tool? Select one response.

 


The data engineer can use the repair feature to re-run only the failed task and the task it depends on.

The data engineer can use the repair feature to re-run only the sub-tasks of the failed task.

The data engineer can use the repair feature to re-run only the failed task and sub-tasks.

The data engineer can use the repair feature to re-run only the failed task and the tasks following it.

The data engineer can use the repair feature to re-run only the failed sub-tasks.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The data engineer can use the repair feature to re-run only the failed task and sub-tasks.
Question 7 of 25
A data engineering team needs to be granted access to metrics on a job run. Each team member has user access without any additional privileges.

 

Which of the following tasks can be performed by an administrator so that each member of the team has access to the metrics? Select one response.

 


The platform administrator can set who can view job results or manage runs of a job with access control lists at the user or group level.

The workspace administrator can set the maximum number of users who can access the table at the group level.

The workspace administrator can set the maximum number of users who can access the table at the user level.

The platform administrator can set who can grant access permissions or view job history with access control lists at the user level.

The platform administrator can set who can search for jobs by id or grant access permissions with access control lists at the user or group level.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The platform administrator can set who can view job results or manage runs of a job with access control lists at the user or group level.
Question 8 of 25
Which of the following statements about the advantages of using Workflows for task orchestration are correct? Select three responses.


Workflows is fully managed, which means users do not need to worry about infrastructure.
(disabled)

Workflows can be used to make external API calls.
(disabled)

Workflows provides a centralized repository for data visualization tools.
(disabled)

Workflows supports built-in data quality constraints for logging purposes.
(disabled)

Workflows can be configured to use external access control permissions.
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
Workflows can be used to make external API calls.
Workflows is fully managed, which means users do not need to worry about infrastructure.
Workflows supports built-in data quality constraints for logging purposes.
Question 9 of 25
A data engineer is running a workflow orchestration on a shared job cluster. They notice that the job they are running is failing and want to use the repair tool to fix the pipeline. 

 

Which of the following statements describes how Databricks assigns a cluster to the repaired job run? Select one response.

 


A new single-user job cluster will be created to run the repair tool on the job run.

The same shared job cluster will be used to run the repair tool on the job run.

The same job cluster will temporarily pause until the job has been repaired. A new job cluster will be created to run the repair tool on the job run.

A new job cluster will be automatically created with the same configuration as the shared job cluster to run the repair tool on the job run.

A new all-purpose job cluster will be created to run the repair tool on the job run.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
A new job cluster will be automatically created with the same configuration as the shared job cluster to run the repair tool on the job run.
Question 10 of 25
A data engineer needs to view whether each task within a job run succeeded.

 

Which of the following steps can the data engineer complete to view this information? Select one response.

 


They can review the task output from the notebook commands.

They can review job run output from the resultant directed acyclic graph (DAG).

They can review the job run history from the Workflow Details page.

They can review the task history by clicking on each task in the workflow.

They can review the job run history from the Job run details page.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
They can review the job run history from the Job run details page.
Question 11 of 25
A data engineer needs their pipeline to run every 12 minutes. 

 

Which of the following approaches automates this process? Select one response.

 


The data engineer can manually pause and start the job every 12 minutes.

The data engineer can use custom Python code to set the job’s schedule.

The data engineer can set the job’s schedule with custom cron syntax.

The data engineer can call the Apache AirFlow API to set the job’s schedule.

This type of scheduling cannot be done with Databricks Workflows.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The data engineer can set the job’s schedule with custom cron syntax.
Question 12 of 25
Which of the following task types can be combined into a single workflow? Select three responses.

 


JAR files
(disabled)

Alert destinations
(disabled)

SQL notebooks
(disabled)

SQL warehouses
(disabled)

Python notebooks
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
JAR files
Python notebooks
SQL notebooks
Question 13 of 25
A data engineer has a notebook that ingests data from a single data source and stores it in an object store. The engineer has three other notebooks that read from the data in the object store and perform various data transformations. The engineer would like to run these three notebooks in parallel after the ingestion notebook finishes running.

 

Which of the following workflow orchestration patterns do they need to use to meet the above requirements? Select one response. 

 


Hourglass pattern

Sequence pattern

Funnel pattern

Multi-sequence pattern

Fan-out pattern
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Fan-out pattern
Question 14 of 25
A data engineer is running multiple notebooks that are triggered on different job schedules. Each notebook is part of a different task orchestration workflow. They want to use a cluster with the same configuration for each notebook.

 

Which of the following describes how the data engineer can use a feature of Workflows to meet the above requirements? Select one response.

 


They can refresh the cluster after each notebook has finished running in order to use the cluster on a new notebook.

They can configure each notebook’s job schedule to swap out the cluster after the job has finished running.

They can use the same cluster to run the notebooks as long as the cluster is a shared cluster.

They can use an alert schedule to swap out the clusters after each job has completed.

They can use a sequence pattern to make the notebooks depend on each other in a task orchestration workflow and run the new workflow on the cluster.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
They can use the same cluster to run the notebooks as long as the cluster is a shared cluster.
Question 15 of 25
A data engineer has a notebook in a remote Git repository. The data from the notebook needs to be ingested into a second notebook that is hosted in Databricks Repos.

 

Which of the following approaches can the data engineer use to meet the above requirements? Select one response.

 


The data engineer can configure the notebook in the remote repository as a task and make it a dependency of the second notebook.

The data engineer can configure the notebook in a new local repository as a job and make the second notebook dependent on it.

The data engineer can configure the notebook in the remote repository as a job and make the second notebook dependent on it.

The data engineer can configure the notebook in a new local remote repository as a job and make it a dependency of the second notebook.

The data engineer can configure the notebook in a new local repository as a task and make the second notebook dependent on it.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The data engineer can configure the notebook in the remote repository as a job and make the second notebook dependent on it.
Question 16 of 25
A data engineer is running a job every 15 minutes. They want to stop the job schedule for an hour before starting it again.

Which of the following allows the data engineer to stop the job during this interval and then start it again without losing the job’s configuration? Select two responses.

 


They can stop the job schedule and then refresh the query within the job after an hour.
(disabled)

They can detach the job from its accompanying dashboard and then reattach and refresh the dashboard after an hour
(disabled)

They can set the Schedule Type to Manual in the Job details panel and change it back to Scheduled after an hour.
(disabled)

They can click Pause in the Job details panel.
(disabled)

They can stop the job schedule and then refresh the notebook that is attached to the task after an hour.
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
They can click Pause in the Job details panel.
They can set the Schedule Type to Manual in the Job details panel and change it back to Scheduled after an hour.
Question 17 of 25
Which of the following are managed by Databricks Workflows? Select three responses.


Table access control lists (ACLs)
(disabled)

Git version control
(disabled)

Error reporting
(disabled)

Cluster management
(disabled)

Task orchestration
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
Task orchestration
Cluster management
Error reporting
Question 18 of 25
A data engineer has a job that creates and displays a result set of baby names by year, where each row has a unique year. They want to display the results for baby names from the past three years only.

 

Which of the following approaches allows them to filter rows from the table by year? Select one response.

 


They can add an import statement to input the year.

They can add widgets to the notebook and re-run the job.

They can edit the table to remove certain rows in the Job Details page.

They can re-run the job with new parameters for the task.

They can add a filter condition to the job’s configuration.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
They can re-run the job with new parameters for the task.
Question 19 of 25
A data engineer has run a Delta Live Tables pipeline and wants to see if there are records that were not added to the target dataset due to constraint violations.

 

Which of the following approaches can the data engineer use to view metrics on failed records for the pipeline? Select two responses.

 


They can view how many records were added to the target dataset from the accompanying SQL dashboard.
(disabled)

They can view information on the percentage of records that succeeded each data expectation from the audit log.
(disabled)

They can view the percentage of records that failed each data expectation from the Pipeline details page.
(disabled)

They can view the duration of each task from the Pipeline details page.
(disabled)

They can view how many records were dropped from the Pipeline details page.
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
They can view how many records were dropped from the Pipeline details page.
They can view the percentage of records that failed each data expectation from the Pipeline details page.
Question 20 of 25
A data engineer has a workload that includes transformations of batch and streaming data, with built-in constraints to ensure each record meets certain conditions.

 

Which of the following task types is considered best practice for the data engineer to use to configure this workload? Select one response.

 


Notebook

Python script

Delta Live Tables pipeline

dbt

Spark submit
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Delta Live Tables pipeline
Question 21 of 25
A data engineer needs to configure the order of tasks to run in their ETL workload. The workload has two tasks, Task A and Task B, where Task B can only be run if Task A succeeds.

 

Which of the following statements describes the dependencies that the data engineer needs to configure and the order they need to be run in? Select one response.

 


They need to add Task B as a subtask of Task A and run the tasks in sequence.

They need to add Task B as a subtask of Task A and run the tasks in parallel.

They need to add Task A as a dependency of Task B and run the tasks in sequence.

They need to add Task B as a dependency of Task A and run the tasks in sequence.

They need to add Task B as a dependency of Task A and run the tasks in parallel.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
They need to add Task B as a dependency of Task A and run the tasks in sequence.
Question 22 of 25
A data engineer has a Python workload they want to run as a job. The code for the workload is located in an external cloud storage location.

 

Which of the following task types and sources can the data engineer use to configure this job? Select one response.

 


Notebook with DBFS source

Python script with DBFS source

Python script with Workspace source

Notebook with local path source

Delta Live Tables pipeline with Workspace source
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Python script with DBFS source
Question 23 of 25
Which of the following tools can be used to create a Databricks Job? Select three responses.


Jobs REST API
(disabled)

Data Explorer
(disabled)

External Git repository
(disabled)

Databricks CLI
(disabled)

Job Scheduler UI
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
Job Scheduler UI
Jobs REST API
Databricks CLI
Question 24 of 25
A data engineer has multiple data sources that they need to combine into one. The combined data sources then need to go through a multi-task ETL process to refine the data using multi-hop (medallion) architecture. It is a requirement that the source data jobs need to be run in parallel.

 

Which of the following workflow orchestration patterns do they need to use to meet the above requirements? Select one response. 

 


Funnel to fan-out pattern

Fan-out to sequence pattern

Sequence to fan-out pattern

Funnel to sequence pattern

Parallel to multi-sequence pattern
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Funnel to sequence pattern
Question 25 of 25
Which of the following configurations are required to specify when scheduling a job? Select two responses. 

 


Trigger type
(disabled)

Start time
(disabled)

Maximum number of runs
(disabled)

Time zone
(disabled)

Trigger frequency
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
Trigger type
Trigger frequency