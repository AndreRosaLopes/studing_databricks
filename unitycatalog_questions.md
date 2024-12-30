Question 1 of 24
A data engineer has a notebook that queries and alters a dynamic view in both Python and SQL. There are no additional libraries that need to be installed on the cluster to run the notebook.

 

Which of the following clusters does the data engineer need to attach to their notebook? Select one response.

 


Single-user cluster

User isolation cluster

This type of workload is not supported by any cluster mode

High concurrency cluster

Multi-user cluster
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
User isolation cluster
Question 2 of 24
Which of the following statements describes the relationship between Unity Catalog and data access control in the overall Databricks Lakehouse architecture? Select one response.


Groups, metastores, and audit control on securables are centrally managed across accounts.

Users, identities, and access control on securables are centrally managed across workspaces.

Accounts, workspaces, and audit control on securables are centrally managed across catalogs.

Identities, groups, and access control on securables are centrally managed across accounts.

Groups, metastores, and access control on securables are centrally managed across workspaces.
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
Groups, metastores, and access control on securables are centrally managed across workspaces.
Question 3 of 24
A data engineer needs to configure their cluster to enable Unity Catalog. They have workspace administrator privileges only.

 

Which of the following steps needs to be completed for the data engineer to enable Unity Catalog on their cluster? Select two responses.

 


The data engineer must have permission to create clusters.
(disabled)

The data engineer must be the data owner of the cluster.
(disabled)

The data engineer must be explicitly granted access to the cluster.
(disabled)

The data engineer must have a storage credential to access the cluster.
(disabled)

The data engineer must use a supported cluster mode.
(disabled)
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answers:
The data engineer must have permission to create clusters.
The data engineer must use a supported cluster mode.
Question 4 of 24
Which of the following data objects can a schema contain in a Unity Catalog namespace? Select three responses.

 


Views
(disabled)

Tables
(disabled)

Storage credentials
(disabled)

Eternal locations
(disabled)

Functions
(disabled)
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answers:
Tables
Functions
Views
Question 5 of 24
A data engineer needs to connect the output of their Unity Catalog-supported Databricks SQL workload to an external BI tool.

 

Which of the following describes what needs to be done to complete this task? Select one response.

 


The data engineer needs to attach their query to an existing all-purpose cluster.

The data engineer needs to attach their query to a new job cluster.

The data engineer needs to attach their query to a new Databricks SQL dashboard.

The data engineer needs to attach their query to a new SQL warehouse.

The data engineer needs to attach their query to a new pipeline.
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
The data engineer needs to attach their query to a new SQL warehouse.
Question 6 of 24
A data engineer has a storage credential whose file path is represented by the variable path. They need to grant the group students permission to query the table at the external location without allowing them to edit the table.

 

Which of the following commands does the data engineer need to run to complete this task? Select one response.

 


GRANT CREATE TABLE ON EXTERNAL LOCATION `${path}` TO `students`;

GRANT WRITE FILES ON EXTERNAL LOCATION `${path}` TO `students`;

GRANT WRITE FILES ON STORAGE CREDENTIAL `${path}` TO `students`;

GRANT READ FILES ON EXTERNAL LOCATION `${path}` TO `students`;

GRANT READ FILES ON STORAGE CREDENTIAL `${path}` TO `students`;
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
GRANT READ FILES ON EXTERNAL LOCATION `${path}` TO `students`;
Question 7 of 24
A metastore administrator needs to create data access control policies for a workspace. They need to provide several users access to a single file in a scalable, efficient way.

 

Which of the following correctly describes the Databricks-recommended best practice to complete this task? Select one response.

 


The metastore administrator can assign access to the file by creating a group of all of the users who need access to the file and assigning access to the group.

None of the provided answer choices are correct.

The metastore administrator can assign access to the file by creating a storage credential and sharing it individually with anyone who needs access to the file.

The metastore administrator can assign access to the file by individually assigning access to each user who needs access to the file.

The metastore administrator can assign access to the file by creating a storage credential and sharing it with a group that includes everyone who needs access to the file.
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
The metastore administrator can assign access to the file by creating a group of all of the users who need access to the file and assigning access to the group.
Question 8 of 24
A lead data engineer needs to create a new workspace for their team. They only have workspace administrator privileges.

 

Which of the following tasks needs to be performed so that the data engineer is granted the necessary permissions to create the workspace? Select one response.

 


The identity administrator needs to assign the data engineer a unique access token to authenticate the platform at an identity administrator level.

The identity administrator needs to assign the data engineer a unique access token to authenticate the platform at an account administrator level.

The identity administrator needs to generate new storage credentials with account administrator level permissions for the data engineer to use

The account administrator needs to generate new storage credentials with account administrator level permissions for the data engineer to use.

The account administrator needs to assign the data engineer a unique access token to authenticate the platform at an identity administrator level.
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
The identity administrator needs to assign the data engineer a unique access token to authenticate the platform at an account administrator level.
Question 9 of 24
Which of the following is a major data governance challenge presented in a traditional data lake backed by cloud storage services? Select one response.


Cloud storage services only provide access control at the file level through cloud-specific interfaces.

Cloud storage services do not support unstructured and semistructured data.

Cloud storage services do not provide scaling for storage costs.

Cloud storage services do not allow access control for groups.

Cloud storage services are usually based on their own proprietary data format, increasing vendor lock-in.
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
Cloud storage services only provide access control at the file level through cloud-specific interfaces.
Question 10 of 24
Which of the following statements correctly identifies the benefits of using a managed table over an external table? Select two responses.


Additional storage credentials are not needed to manage access to the underlying cloud storage for a managed table.
(disabled)

External tables support multiple formats, while managed tables only support Delta format.
(disabled)

Unity Catalog supports managed tables, but does not support external tables.
(disabled)

When managed tables are dropped, the underlying data and metadata are dropped as well.
(disabled)

Managed tables support multiple formats, while external tables only support external formats.
(disabled)
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answers:
Additional storage credentials are not needed to manage access to the underlying cloud storage for a managed table.
When managed tables are dropped, the underlying data and metadata are dropped as well.
Question 11 of 24
In which of the following locations are the metadata related to metastore-managed data objects (like a table’s columns and data types) stored? Select one response.


A separate cloud storage container specific to the Databricks workspace

A separate external database outside of Databricks

A separate cloud storage container in the control plane

A separate cloud storage container in the data plane

A separate external database managed by Databricks
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
A separate cloud storage container in the control plane
Question 12 of 24
A data engineer needs to copy an external table from their default Hive metastore to the Unity Catalog metastore.

 

Which of the following are required to upgrade the table to be managed by Unity Catalog? Select three responses. 

 


The data engineer must have workspace administrator level privileges.
(disabled)

The data engineer must be granted CREATE EXTERNAL TABLE permission on the external location of the table to be upgraded.
(disabled)

The data engineer must have the file path to an external location that references the storage credential.
(disabled)

The data engineer must have a storage credential with an IAM role that authorizes Unity Catalog to access the tables’ location path.
(disabled)

The data engineer must create their own Hive metastore.
(disabled)
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answers:
The data engineer must have a storage credential with an IAM role that authorizes Unity Catalog to access the tables’ location path.
The data engineer must have the file path to an external location that references the storage credential.
The data engineer must be granted CREATE EXTERNAL TABLE permission on the external location of the table to be upgraded.
Question 13 of 24
A data engineer needs to upgrade the Delta table records_silver from the old schema records to a Unity Catalog table updated_silver within the catalog customers and the new schema updated_records. 

Which of the following queries correctly upgrades the table to be managed by Unity Catalog? Select one response.

 


CREATE TABLE customers.records.records_silver
AS SELECT * FROM hive_metastore.records.records_silver;

CREATE TABLE customers.records.records_silver
AS SELECT * FROM hive_metastore.customers.records;

CREATE TABLE records.customers.records_silver
AS SELECT * FROM hive_metastore.records.records_silver;

CREATE TABLE customers.updated_records.updated_silver
AS SELECT * FROM hive_metastore.records.records_silver;

CREATE TABLE records.customers.records_silver

AS SELECT * FROM hive_metastore.records_silver.records;
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
CREATE TABLE customers.updated_records.updated_silver
AS SELECT * FROM hive_metastore.records.records_silver;
Question 14 of 24
Which of the following privileges do storage credentials AND external locations support? Select three responses.


DELETE
(disabled)

CREATE EXTERNAL TABLE
(disabled)

WRITE FILES
(disabled)

READ FILES
(disabled)

EXECUTE
(disabled)
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answers:
CREATE EXTERNAL TABLE
READ FILES
WRITE FILES
Question 15 of 24
A data engineering team has members at several different geographic locations. Each member of the team needs to be able to access the securables in the team's Unity Catalog namespace from their location. 

 

How can the data be managed in a way that each team member’s region has access to the securables within the catalog? Select one response.

 


The metastore administrator needs to create one metastore to be used in all regions.

The metastore administrator needs to create a metastore for each region.

The account administrator needs to create a metadata layer for each region.

The account administrator needs to create a metadata layer to be used in all regions.

The metastore administrator needs to create a catalog for each region.
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
The metastore administrator needs to create a metastore for each region.
Question 16 of 24
A metastore administrator has enabled identity federation for a Unity Catalog namespace.

Which of the following correctly describes the privileges that users who have access to the catalog now have? Select two responses.

 


The users can be assigned from the account to the workspace by the workspace administrator through their workspace administration console.
(disabled)

The users can be assigned from the workspace to the account by an account administrator through the account console.
(disabled)

The users can be assigned from the account to the workspace by the account administrators through the account console.
(disabled)

The users can be assigned from the workspace to the account by a workspace user through the identity provider.
(disabled)

The users can be assigned from the workspace to the account by the workspace administrator through their workspace administration console.
(disabled)
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answers:
The users can be assigned from the account to the workspace by the account administrators through the account console.
The users can be assigned from the account to the workspace by the workspace administrator through their workspace administration console.
Question 17 of 24
Which of the following security modes supports Unity Catalog? Select two responses.


Table ACL only mode
(disabled)

None (no security)
(disabled)

Passthrough only mode
(disabled)

User isolation mode
(disabled)

Single user mode
(disabled)
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answers:
Single user mode
User isolation mode
Question 18 of 24
A data engineering team is working to migrate some existing data files into Delta format. For the time being, they need to be able to query those files in their original data format.

 

Which of the following tables does the data engineering team need to use and why? Select one response.

 


Managed tables; Unity Catalog retains the underlying data for up to 30 days.

External tables; external tables support Delta and other formats.

Managed tables; Unity Catalog does not drop the underlying data for managed tables.

Managed tables; only managed tables support Delta format.

External tables; only external tables support Delta format.
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
External tables; external tables support Delta and other formats.
Question 19 of 24
Which of the following describes an additional benefit of the three-level namespace provided by Unity Catalog? Select one response.

 


The three-level namespace enforces data governance through cluster modes over data objects in the Hive metastore.

The three-level namespace provides access to advanced options to optimize data through versioning techniques.

The three-level namespace enforces a list of privilege grants for each securable data object in the Hive metastore.

The three-level namespace allows implicit access grants so permissions can easily be inherited by securable data objects.

The three-level namespace provides more data segregation options while still making legacy Hive metastore data easily accessible.
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
The three-level namespace provides more data segregation options while still making legacy Hive metastore data easily accessible.
Question 20 of 24
Which of the following describes the query lifecycle within the context of Unity Catalog? Select one response.


The transfer of data between the namespace, metadata layer, and service principal.

The transfer of data between the service principal, compute resources, and audit log.

The transfer of data between the service principal, groups, and cloud storage.

The transfer of data between the data principal, storage credentials, and cloud storage.

The transfer of data between the data principal, compute resources, and cloud storage.
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
The transfer of data between the data principal, compute resources, and cloud storage.
Question 21 of 24
Which of the following lists the four key functional areas of data governance? Select one response.


Data integrity, data optimization, data lineage, data science

Data science, data integrity, data lineage, data versioning

Data access control, data access audit, data lineage, data discovery

Data optimization, data access audit, data analysis, data discovery

Data history, data access control, data integrity, data validation
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
Data access control, data access audit, data lineage, data discovery
Question 22 of 24
Which of the following is a recommended best practice to segregate data within an organization? Select one response


It is recommended to segregate data using clusters.

It is recommended to segregate data using tables.

It is recommended to segregate data using catalogs.

It is recommended to segregate data using schemas.

It is recommended to segregate data using metastores.
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
It is recommended to segregate data using catalogs.
Question 23 of 24
A data engineer needs to modify a table’s data and metadata. They are not the owner of the table.

 

Which of the following steps need to be completed in order for the data engineer to make modifications to the table data and metadata? Select three responses.

 


The data engineer needs to be granted USAGE permissions on the table.
(disabled)

The data engineer needs to be granted MODIFY permissions on the table.
(disabled)

The data engineer needs to be granted SELECT permissions to modify the table data.
(disabled)

The data engineer needs to use the ALTER command to modify the table metadata.
(disabled)

The data engineer needs to use the INSERT and/or DELETE commands to modify the table data.
(disabled)
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answers:
The data engineer needs to be granted MODIFY permissions on the table.
The data engineer needs to use the ALTER command to modify the table metadata.
The data engineer needs to use the INSERT and/or DELETE commands to modify the table data.
Question 24 of 24
A data engineer is reading data from multiple external sources. They are only writing the data to one local file. 

 

Which of the following recommendations is considered a best practice in this situation? Select one response.

 


Keep the table managed and use Delta Sharing to capture upstream sources only.

Keep the table managed and use Delta Sharing for external consumption.

Move the table to an external location and use Delta Sharing for external consumption.

Make a copy of the table in an external location and use Delta Sharing for external consumption.

Move the table to an external location and use Delta Sharing to capture upstream sources only.
Options
You gave the wrong answer and your score is 0
Wrong answer
Score: 0
Correct answer:
Keep the table managed and use Delta Sharing for external consumption.