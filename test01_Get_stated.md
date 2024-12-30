Question 1 of 21
Which of the following operations are supported by Databricks Repos? Select two responses.


Sync
(disabled)

Pull
(disabled)

Rebase
(disabled)

Reset
(disabled)

Clone
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
Clone
Pull
Question 2 of 21
A data engineer is trying to merge their development branch into the main branch for a data project's repository.

 

Which of the following is a correct argument for why it is advantageous for the data engineering team to use Databricks Repos to manage their notebooks? Select one response.

 


Databricks Repos provides access to available data sets and data sources, on-premises or in the cloud.

Databricks Repos REST API enables the integration of data projects into CI/CD pipelines.

Databricks Repos uses one common security model to access each individual notebook, or a collection of notebooks, and experiments.

Databricks Repos allows integrations with popular tools such as Tableau, Looker, Power BI, and RStudio.

Databricks Repos provides a centralized, immutable history that cannot be manipulated by users.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Databricks Repos REST API enables the integration of data projects into CI/CD pipelines.
Question 3 of 21
Which of the following resources reside in the data plane of a Databricks deployment? Select one response.


Notebooks

Cluster manager

Web application

Databricks File System (DBFS)

Job scheduler
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Databricks File System (DBFS)
Question 4 of 21
A data engineer wants to stop running a cluster without losing the cluster’s configuration. The data engineer is not an administrator.

 

Which of the following actions can the data engineer take to satisfy their requirements and why? Select one response.

 


Terminate the cluster; clusters are retained for 30 days after they are terminated.

Edit the cluster; clusters can be saved as templates in the cluster configuration page before they are deleted.

Delete the cluster; clusters are retained for 30 days after they are deleted.

Detach the cluster; clusters are retained for 70 days after they are detached from a notebook.

Delete the cluster; clusters are retained for 60 days after they are deleted.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Terminate the cluster; clusters are retained for 30 days after they are terminated.
Question 5 of 21
Due to the platform administrator’s policies, a data engineer needs to use a single cluster on one very large batch of files for an ETL workload. The workload is automated, and the cluster will only be used by one workload at a time. They are part of an organization that wants them to minimize costs when possible.


Which of the following cluster configurations can the team use to satisfy their requirements? Select one response.


Multi node job cluster

High concurrency all-purpose cluster

Multi node all-purpose cluster

Single node all-purpose cluster

Single node job cluster
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Multi node job cluster
Question 6 of 21
Three data engineers are collaborating on a project using a Databricks Repo. They are working on the notebook at separate times of the day. 

 

Which of the following is considered best practice for collaborating in this way? Select one response.

 


The engineers can each create their own Databricks Repo for development and merge changes into a main repository for production.

The engineers can each work in their own branch for development to avoid interfering with each other.

The engineers can use a separate internet-hosting service to develop their code in a single repository before merging their changes into a Databricks Repo.

The engineers can set up an alert schedule to notify them when changes have been made to their code.

The engineers can each design, develop, and trigger their own Git automation pipeline.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The engineers can each work in their own branch for development to avoid interfering with each other.
Question 7 of 21
A data engineer needs the results of a query contained in the third cell of their notebook. It has been verified by another engineer that the query runs correctly. However, when they run the cell individually, they notice an error. 

 

Which of the following steps can the data engineer take to ensure the query runs without error? Select two responses.

 


The data engineer can clear the execution state before re-executing the cell individually.
(disabled)

The data engineer can choose “Run all below” from the dropdown menu within the cell.
(disabled)

The data engineer can clear all cell outputs before re-executing the cell individually.
(disabled)

The data engineer can run the notebook cells in order starting from the first command.
(disabled)

The data engineer can choose “Run all above” from the dropdown menu within the cell.
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
The data engineer can run the notebook cells in order starting from the first command.
The data engineer can choose “Run all above” from the dropdown menu within the cell.
Question 8 of 21
A data engineering team is working on a shared repository. Each member of the team has cloned the target repository and is working in a separate branch.

 

Which of the following is considered best practice for the team members to commit their changes to the centralized repository? Select one response.

 


The data engineers can each call the Databricks Repos API to submit the code changes for review before they are merged into the main branch.

The data engineers can each sync their changes with the main branch from the Git terminal, which will automatically commit their changes.

The data engineers can each run a job based on their branch in the Production folder of the shared repository so the changes can be merged into the main branch.

The data engineers can each commit their changes to the main branch using an automated pipeline after a thorough code review by other members of the team.

The data engineers can each create a pull request to be reviewed by other members of the team before merging the code changes into the main branch.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The data engineers can each create a pull request to be reviewed by other members of the team before merging the code changes into the main branch.
Question 9 of 21
A data engineer is working on an ETL pipeline. There are several utility methods needed to run the notebook, and they want to break them down into simpler, reusable components.

 

Which of the following approaches accomplishes this? Select one response.

 


Create a separate job for the utility commands and run the job from within the original notebook using the %cmd magic command.

Create a separate notebook for the utility commands and use an import statement at the beginning of the original notebook to reference the notebook with the utility commands.

Create a pipeline for the utility commands and run the pipeline from within the original notebook using the %md magic command.

Create a separate notebook for the utility commands and use the %run magic command in the original notebook to run the notebook with the utility commands.

Create a separate task for the utility commands and make the notebook dependent on the task from the original notebook’s Directed Acyclic Graph (DAG).
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Create a separate notebook for the utility commands and use the %run magic command in the original notebook to run the notebook with the utility commands.
Question 10 of 21
Which of the following cluster configuration options can be customized at the time of cluster creation? Select all that apply.


Cluster mode
(disabled)

Access permissions
(disabled)

Maximum number of worker nodes
(disabled)

Restart policy
(disabled)

Databricks Runtime Version
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
Databricks Runtime Version
Cluster mode
Maximum number of worker nodes
Question 11 of 21
A data architect is proposing that their organization migrate from a data lake to a data lakehouse. The architect claims that this will improve and simplify the work of the data engineering team.

 

Which of the following describes the key benefits of why migrating from a data lake to a data lakehouse will be beneficial for the data engineering team? Select two responses.

 


Data lakehouses are able to improve query performance by managing metadata and utilizing advanced data partitioning techniques.
(disabled)

Data lakehouses are able to support data quality solutions like ACID-compliant transactions.
(disabled)

Data lakehouses are able to support programming languages like Python.
(disabled)

Data lakehouses are able to support cost-effective scaling.
(disabled)

Data lakehouses are able to support machine learning workloads.
(disabled)
Options
You gave a partially correct answer and your score is 5
Partially correct answer
Score: 5
Correct answers:
Data lakehouses are able to improve query performance by managing metadata and utilizing advanced data partitioning techniques.
Data lakehouses are able to support data quality solutions like ACID-compliant transactions.
Question 12 of 21
A data engineer needs to run some SQL code within a Python notebook. Which of the following will allow them to do this? Select two responses.


They can run the import sql statement at the beginning of their notebook.
(disabled)

It is not possible to run SQL code from a Python notebook.
(disabled)

They can use the %md command at the top of the cell containing SQL code.
(disabled)

They can wrap the SQL command in spark.sql().
(disabled)

They can use the %sql command at the top of the cell containing SQL code.
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
They can use the %sql command at the top of the cell containing SQL code.
They can wrap the SQL command in spark.sql().
Question 13 of 21
A data engineer has a long-running cluster for an ETL workload. Before the next time the workload runs, they need to ensure that the image for the compute resources is up-to-date with the latest image version.

 

Which of the following cluster operations can be used in this situation? Select one response.

 


Delete

Start

Restart

Edit

Terminate
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Restart
Question 14 of 21
Two data engineers are collaborating on one notebook in the same repository. Each is worried that if they work on the notebook at different times, they might overwrite changes that the other has made to the code within the notebook.

 

Which of the following explains why collaborating in Databricks Notebooks prevents these problems from occurring? Select one response.

 


Databricks Notebooks automatically handles schema variations to prevent insertion of bad records during ingestion, so the data engineers will be prevented from overwriting data that does not match the table’s schema.

Databricks Notebooks supports alerts and audit logs for easy monitoring and troubleshooting, so the data engineers will be alerted when changes are made to their code.

Databricks Notebooks supports real-time co-authoring, so the data engineers can work on the same notebook in real-time while tracking changes with detailed revision history.

Databricks Notebooks are integrated into CI/CD pipelines by default, so the data engineers can work in separate branches without overwriting the other’s work.

Databricks Notebooks enforces serializable isolation levels, so the data engineers will never see inconsistencies in their data.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Databricks Notebooks supports real-time co-authoring, so the data engineers can work on the same notebook in real-time while tracking changes with detailed revision history.
Question 15 of 21
A data engineer needs to develop an interactive dashboard that displays the results of a query.

 

Which of the following services can they employ to accomplish this? Select one response.

 


Databricks SQL

Databricks Machine Learning

Unity Catalog

Delta Live Tables (DLT)

Delta Lake
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Databricks SQL
Question 16 of 21
Which of the following statements describes how to clear the execution state of a notebook? Select two responses.


Perform a Clean operation from the terminal.
(disabled)

Use the Clear State option from the Run dropdown menu.
(disabled)

Perform a Clean operation from the driver logs.
(disabled)

Perform a Clear State operation from the Spark UI.
(disabled)

Detach and reattach the notebook to a cluster.
(disabled)
Options
You gave a partially correct answer and your score is 5
Partially correct answer
Score: 5
Correct answers:
Detach and reattach the notebook to a cluster.
Use the Clear State option from the Run dropdown menu.
Question 17 of 21
Which of the following resources reside in the control plane of a Databricks deployment? Select two responses.


Job scheduler
(disabled)

Job configurations
(disabled)

Notebook commands
(disabled)

JDBC and SQL data sources
(disabled)

Databricks File System (DBFS)
(disabled)
Options
You gave a partially correct answer and your score is 5
Partially correct answer
Score: 5
Correct answers:
Notebook commands
Job scheduler
Question 18 of 21
Which of the following pieces of information must be configured in the user settings of a workspace to integrate a Git service provider with a Databricks Repo? Select two responses.


Personal Access Token
(disabled)

Username for Git service provider account
(disabled)

Workspace Access Token
(disabled)

Two-factor authentication code from Git service provider
(disabled)

Administrator credentials for Git service provider account
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
Username for Git service provider account
Personal Access Token
Question 19 of 21
Which of the following correctly lists the programming languages that Databricks Notebooks can have set as the default programming language? Select one response.


HTML, Python, R, Scala

Bash script, Python, Scala, SQL

Python, R, Scala, SQL

Java, Pandas, Python, SQL

HTML, Python, R, SQL
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Python, R, Scala, SQL
Question 20 of 21
A data engineer is creating a multi-node cluster.

 

Which of the following statements describes how workloads will be distributed across this cluster? Select one response.

 


Workloads are distributed across available worker nodes by the driver node.

Workloads are distributed across available compute resources by the executor.

Workloads are distributed across available memory by the executor.

Workloads are distributed across available driver nodes by the worker node.

Workloads are distributed across available worker nodes by the executor.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
Workloads are distributed across available worker nodes by the driver node.
Question 21 of 21
Which of the following are data objects that can be created in the Databricks Data Science and Engineering workspace? Select two responses.


Tables
(disabled)

MLflow Models
(disabled)

Functions
(disabled)

Clusters
(disabled)

SQL Warehouses
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
Tables
Functions