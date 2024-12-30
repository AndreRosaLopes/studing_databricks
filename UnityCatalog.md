# Manage Data Access with Unity Catalog
## Introdution
### 6.2. Data Governance
Data governance referes to the process of managing the availability, usability, integrity and securty of data.
* Data Access Control
* Data Access Audit - capture and record all access to data
* Data Lineage
* Data Discovery - ability to search for and discover authorized assets

![image](UnityCatalog\data_gov_overview.png)

![image](UnityCatalog\UC_overview.png)

### 6.3. Unity Catalog Key Concepts

Hive Metastore - linked to each databricks workspace.
+ improved security model + auditing capabilities
= Unity Catalog

![image](UnityCatalog\metastore.png)

Three-level namespace: ```sql
 SELECT * CATALOG.SCHEMA.OBJECT ```

 * Managed table
 * External table
 * View
 * Function

### 6.4. Archiecture

![image](UnityCatalog\architecture.png)

In the tradicional way (before the Unity Catalog) full security requires cooperation between compute resources and the metastore. Your clusters must be configured to follow access control rules in order for access control to do not bypassed.

### 6.5. Unity Catalog Identities

email + first + lastname

* Groups maps rules, grants, roles
* identity federation

### 6.6. Managing Principals in Unity Catalog

There are 2 levels: account-level identitties and (individual?)

There are managed through the Databricks account console or its associated SCIM APIs.



### 6.7. Managing Catalog Metastores
video:
* Create a metastore
* Set to a group for adm

## Compute Resources and Unity Catalog

### 6.8. Cluster Security Mode
Modes supporting Unity Catalog:
* Single user - not shareable
* User isolation - sharealbe, python and SQL, legacy table ACLs. It does not support library installation, init scripts and DBFS fuse mounts.

Modes not supportin Unity Catalog:
* None - no security
* Table ACL only - legacy table ACLs, multiple languages, shareable
* Passthrough only - credential passthrough, multiple langaguages, shareable

![alt text](UnityCatalog\cluster_security_mode.png)

### 6.9. Creating compute sources for Unity Catalog access
<notebook 6.1.3>

### 6.10. Data Access Control in Databricks

![alt text](UnityCatalog\legacy_hive.png)

Legacy data objects are just as readily accessible as any objects within Unity Catalog's control. Note that Unity Catalog itself will not provide any data governance over objects within Hive metastore. 

Access control on th Hive metastore can only be accomplished by choosing an appropriate access mode for your cluster.

### 6.11. Unity Catalog Security Model

![alt text](UnityCatalog\security_model.png)

Only account admin are able to create catalogs.


Create // Usage => Catalog + Schema

Select // Modify => Table 

Select => View (users can have access to a view without having access to underline tables). It a way to garantee a safe way to give access (filtering data).

Execute => Function

Create Table // Read files // Write files => Storege credential + External location

* Share
* Recipient

* Access to a table needs

**Usage** of Catalog and Schema

**Select/Modify** of Table

Dynamic Views

It is made to build a securety layer over a table:
1. Limiting access to columns
2. Limiting access to rows
3. Data masking: *****@databricks.com

* Create new objects

Usage => Catalog
Usage + Create => Schema

* Drop objects

Only can be done by the owner or accounting administrator.

### 6.12. External Storage

Store Credential

Enables Unity Catalog to connect to an external cloud store. Examples:

- IAM role for AWS S3
- Service principal for Azure Storage

External Location

Cloud Storage path + storage credential
- Self-contained object for accessing specific locations in cloud in cloud stores
- Fine-grained control over external storage

![alt text](UnityCatalog\external_access.png)

### 6.13. Creating and Governing Data
<DE 6.2 notebook>

Unity Catalog's three-level namespace

    Anyone with SQL experience will likely be familiar with the traditional two-level namespace to address tables or views within a schema as follows, as shown in the following example query:

        SELECT * FROM myschema.mytable;

    Unity Catalog introduces the concept of a **catalog** into the hierarchy. As a container for schemas, the catalog provides a new way for organizations to segregate their data. There can be as many catalogs as you like, which in turn can contain as many schemas as you like (the concept of a **schema** is unchanged by Unity Catalog; schemas contain data objects like tables, views, and user-defined functions).

    To deal with this additional level, complete table/view references in Unity Catalog use a three-level namespace. The following query exemplifies this:

        SELECT * FROM mycatalog.myschema.mytable;

    This can be handy in many use cases. For example:

    * Separating data relating to business units within your organization (sales, marketing, human resources, etc)
    * Satisfying SDLC requirements (dev, staging, prod, etc)
    * Establishing sandboxes containing temporary datasets for internal use

**Create a new catalog**
```sql
CREATE CATALOG IF NOT EXISTS ${DA.my_new_catalog}
```
**Select a default catalog**

    USE CATALOG mycatalog;
    USE SCHEMA myschema;  
    
```sql
USE CATALOG ${DA.my_new_catalog}
```

```sql
CREATE SCHEMA IF NOT EXISTS example;
USE SCHEMA example
```

```sql
CREATE OR REPLACE TABLE heartrate_device (device_id INT, mrn STRING, name STRING, time TIMESTAMP, heartrate DOUBLE);

INSERT INTO heartrate_device VALUES
  (23, "40580129", "Nicholas Spears", "2020-02-01T00:01:58.000+0000", 54.0122153343),
  (17, "52804177", "Lynn Russell", "2020-02-01T00:02:55.000+0000", 92.5136468131),
  (37, "65300842", "Samuel Hughes", "2020-02-01T00:08:58.000+0000", 52.1354807863),
  (23, "40580129", "Nicholas Spears", "2020-02-01T00:16:51.000+0000", 54.6477014191),
  (17, "52804177", "Lynn Russell", "2020-02-01T00:18:08.000+0000", 95.033344842);
  
SELECT * FROM heartrate_device
```
```sql
-- Creating a gold view to share with other users

CREATE OR REPLACE VIEW agg_heartrate AS (
  SELECT mrn, name, MEAN(heartrate) avg_heartrate, DATE_TRUNC("DD", time) date
  FROM heartrate_device
  GROUP BY mrn, name, DATE_TRUNC("DD", time)
);
SELECT * FROM agg_heartrate
```
In accounts with Unity Catalog enabled, there is an _account users_ group. This group contains all users that have been assigned to the workspace from the Databricks account. We are going to use this group to show how data object access can be different for users in different groups.

Unity Catalog employs an explicit permission model by default; no permissions are implied or inherited from containing elements. Therefore, in order to access any data objects, users will need **USAGE** permission on all containing elements; that is, the containing schema and catalog.

```sql
GRANT USAGE ON CATALOG ${DA.my_new_catalog} TO `account users`;
GRANT USAGE ON SCHEMA example TO `account users`;
GRANT SELECT ON VIEW agg_heartrate to `account users`;
```
```sql
SELECT "SELECT * FROM ${DA.my_new_catalog}.example.agg_heartrate" AS Query
--results:
SELECT * FROM labuser8570626_1735391878_kqy2_da.example.agg_heartrate
```

**Query the silver table**
    Back in the same query in the Databricks SQL session, let's replace *gold* with *silver* and run the query. As the data owner, running this should return all table results as expected. However, if you were to run this as another user, this would fail because we never set up permissions on the *silver* table. 

    Querying *gold* works for all members of the _account users_ group because the query represented by a view is essentially executed as the owner of the view. This important property enables some interesting security use cases; in this way, views can provide users with a restricted view of sensitive data, without providing access to the underlying data itself. We will see more of this shortly.

    For now, you can close and discard the *silver* query in the Databricks SQL session; we will not be using it any more.

```sql
-- Create and grant access to a user-defined function
CREATE OR REPLACE FUNCTION my_mask(x STRING)
  RETURNS STRING
  RETURN CONCAT(REPEAT("*", LENGTH(x) - 2), RIGHT(x, 2)
); 
SELECT my_mask('sensitive data') AS data
```

```sql
-- To allow members of the account users group to run our function, they need EXECUTE on the function, along with the requisite USAGE grants on the schema and catalog that we've mentioned before.
GRANT EXECUTE ON FUNCTION my_mask to `account users`
```
**Protect table columns and rows with dynamic views**
    Dynamic views provide the ability to do fine-grained access control of columns and rows within a table, conditional on the principal running the query. Dynamic views are an extension to standard views that allow us to do things like:
    * partially obscure column values or redact them entirely
    * omit rows based on specific criteria

    Access control with dynamic views is achieved through the use of functions within the definition of the view. These functions include:
    * **`current_user()`**: returns the email address of the user querying the view
    * **`is_account_group_member()`**: returns TRUE if the user querying the view is a member of the specified group

    Note: please refrain from using the legacy function **`is_member()`**, which references workspace-level groups. This is not good practice in the context of Unity Catalog.

```sql
-- Redact columns
CREATE OR REPLACE VIEW agg_heartrate AS
SELECT
  CASE WHEN
    is_account_group_member('account users') THEN 'REDACTED'
    ELSE mrn
  END AS mrn,
  CASE WHEN
    is_account_group_member('account users') THEN 'REDACTED or any other desired text'
    ELSE name 
  END AS name,
  MEAN(heartrate) avg_heartrate,
  DATE_TRUNC("DD", time) date
  FROM heartrate_device
  GROUP BY mrn, name, DATE_TRUNC("DD", time)

-- Note: this is a simple training example that doesn't necessarily align with general best practices. For a production system, a more secure approach would be to redact column values for all users who are not members of a specific group.


-- Grant:
GRANT SELECT ON VIEW agg_heartrate to `account users`
```

```sql
-- Restrict rows
CREATE OR REPLACE VIEW agg_heartrate_rows AS
SELECT
  mrn,
  time,
  device_id,
  heartrate
FROM heartrate_device
WHERE
  CASE WHEN
    is_account_group_member('account users') THEN device_id < 30
    ELSE TRUE
  END
```

```sql
-- Data masking + Restrict rows
DROP VIEW IF EXISTS agg_heartrate_masking;

CREATE VIEW agg_heartrate_masking AS
SELECT
  CASE WHEN
    is_account_group_member('account users') THEN my_mask(mrn)
    ELSE mrn
  END AS mrn,
  time,
  device_id,
  heartrate
FROM heartrate_device
WHERE
  CASE WHEN
    is_account_group_member('account users') THEN device_id < 30
    ELSE TRUE
  END
```
*Revoke Access**
```sql
REVOKE EXECUTE ON FUNCTION my_mask FROM `account users`
REVOKE USAGE ON CATALOG ${DA.my_new_catalog} FROM `account users`
```

### 6.14. Create and Share Tables in Unity Catalog
**Create and use a new schema**
```sql
CREATE SCHEMA IF NOT EXISTS my_own_schema;
USE my_own_schema;

SELECT current_database()
```
**Create Delta architecture**
```sql
-- Silver
CREATE SCHEMA IF NOT EXISTS patient_silver;

CREATE OR REPLACE TABLE patient_silver.heartrate (
  device_id  INT,
  mrn        STRING,
  name       STRING,
  time       TIMESTAMP,
  heartrate  DOUBLE
);

INSERT INTO patient_silver.heartrate VALUES
  (23,'40580129','Nicholas Spears','2020-02-01T00:01:58.000+0000',54.0122153343),
  (17,'52804177','Lynn Russell','2020-02-01T00:02:55.000+0000',92.5136468131),
  (37,'65300842','Samuel Hughes','2020-02-01T00:08:58.000+0000',52.1354807863),
  (23,'40580129','Nicholas Spears','2020-02-01T00:16:51.000+0000',54.6477014191),
  (17,'52804177','Lynn Russell','2020-02-01T00:18:08.000+0000',95.033344842),
  (37,'65300842','Samuel Hughes','2020-02-01T00:23:58.000+0000',57.3391541312),
  (23,'40580129','Nicholas Spears','2020-02-01T00:31:58.000+0000',56.6165053697),
  (17,'52804177','Lynn Russell','2020-02-01T00:32:56.000+0000',94.8134313932),
  (37,'65300842','Samuel Hughes','2020-02-01T00:38:54.000+0000',56.2469995332),
  (23,'40580129','Nicholas Spears','2020-02-01T00:46:57.000+0000',54.8372685558)

-- Gold
CREATE SCHEMA IF NOT EXISTS patient_gold;

CREATE OR REPLACE TABLE patient_gold.heartrate_stats AS (
  SELECT mrn, name, MEAN(heartrate) avg_heartrate, DATE_TRUNC("DD", time) date
  FROM patient_silver.heartrate
  GROUP BY mrn, name, DATE_TRUNC("DD", time)
);
  
SELECT * FROM patient_gold.heartrate_stats;
```
Grant access to gold schema to 'account useres'
```sql
GRANT SELECT ON TABLE patient_gold.heartrate_stats to `account users`
```
```python
print(f"SELECT * FROM {DA.catalog_name}.patient_gold.heartrate_stats")
```

```sql
GRANT USAGE ON CATALOG ${DA.catalog_name} TO `account users`;
GRANT USAGE ON SCHEMA patient_gold TO `account users`
```


### 6.15. Create external tables in Unity Catalog
<De 6.4 notebook>


### 6.16. Best Practices
Unity Catalog Patterns and Best Practices

* Metastores
One metastore per region (as Unity Catalog offers a low latency metadata layer).

Metastores can share data if needed, using Databricks-to-Databricks Delta Sharing (also can do it with multiples).

Access control lists do not cross Delta Sharing. So, access control rules need to be set up in the destination.

### 6.17. Data Segregation
![alt text](UnityCatalog\data_segregation.png)

### 6.18. Identity Management

Access Control List (ACL)

Identity providers, or the SCIM API, should not be used at the workspace level at all.

Grant access with Account to workspace-level.

Grant access to Groups to object. Groups should always be synchronized with the system and they are managed in via SCIM API or through your identity provider.

It is vest practice to use service principals to run production jobs. Thhis way if the user's removed, there are no unforeseen impacts to running jobs. Furthermore, users should not run jobs that write into production. This reduces the risk of users overwriting production data by accident. The flip side of this is that users should never be granted MODIFY access to production tables.

### 6.19. External Store

* Storage Credential - enables Unity Catalog to connect to an external cloud store
* External Location - cloud storage path + storeage credential
    - Self-contained object for accessing specific locations in cloud stores
    - Fine-grained control over external storage

Recapping:

![alt text](UnityCatalog\ManagedXexternal_tables.png)

When we need external tables?

* Quick and easy upgrade from external table in Hive metastore
* External readers or writers (If we have just external readers it is recommend keep the table managed and use Delta sharing for external consumption)
* Requirement for specific storage naming or hierarchy
* Infrastructure-level isolation requirements
* Non-Delta support requirement

### 6.20. Upgrade a Table to Unity Catalog
<notebook DE 6.5 - Upgrade a Table to Unity Catalog>

### 6.21. Creating Views and Limiting Table Access
<notebook DE 6.6>





