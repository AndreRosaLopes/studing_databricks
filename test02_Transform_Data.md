Question 1 of 20
Which of the following statements about the difference between views and temporary views are correct? Select two responses.


Temporary views are session-scoped and dropped when the Spark session ends. Views can be accessed after the session ends.
(disabled)

Temporary views have names that must be qualified. Views have names that must be unique.
(disabled)

Temporary views skip persisting the definition in the underlying metastore. Views have metadata that can be accessed in the view’s directory.
(disabled)

Temporary views reside in the third layer of Unity Catalog’s three-level namespace Views lie in the metastore.
(disabled)

Temporary views do not contain a preserved schema. Views are tied to a system preserved temporary schema global_temp.
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
Temporary views are session-scoped and dropped when the Spark session ends. Views can be accessed after the session ends.
Temporary views skip persisting the definition in the underlying metastore. Views have metadata that can be accessed in the view’s directory.
Question 2 of 20
A data engineer is using the following query to confirm that each unique string value in the phone_number column in the usersDF DataFrame is associated with at most one user_id. They want the query to return true if each phone_number is associated with at most 1 user_id. When they run the query, they notice that the query is not returning the expected result. 

 

Code block:

from pyspark.sql.functions import countDistinct

 

usersDF

    .groupBy("phone_number")

    .agg(countDistinct("user_id").alias("unique_user_ids")) 

 

Which of the following explains why the query is not returning the expected result? Select one response.

 


A .merge statement on row_count == count(phone_number) needs to be added after the groupBy() function.

.groupBy("phone_number") needs to be changed to .countDistinct(phone_number).

A .dropDuplicates() statement needs to be added after the .agg() function.

.groupBy("phone_number") needs to be changed to count(*).when(user_id != null).

A .select(max("unique_user_ids") <= 1)function needs to be added after the .agg() function.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
A .select(max("unique_user_ids") <= 1)function needs to be added after the .agg() function.
Question 3 of 20
Which of the following statements about querying tables defined against external sources is true? Select one response.

 


None of these statements about external table behavior are true.

When defining tables or queries against external data sources, older cached versions of the table are automatically deleted.

When defining tables or queries against external data sources, older cached versions of the table are automatically added to the event log.

When defining tables or queries against external data sources, the performance guarantees associated with Delta Lake and Lakehouse cannot be guaranteed.

When defining tables or queries against external data sources, the storage path, external location, and storage credential are displayed for users who have been granted USAGE access to the table.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
When defining tables or queries against external data sources, the performance guarantees associated with Delta Lake and Lakehouse cannot be guaranteed.
Question 4 of 20
Which of the following lines of code counts null values in the column email from the DataFrame usersDF? Select two responses.


usersDF.distinct()
(disabled)

usersDF.count().dropna()
(disabled)

usersDF.where(col("email").isNull()).count()
(disabled)

usersDF.drop()
(disabled)

usersDF.selectExpr("count_if(email IS NULL)")
(disabled)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answers:
usersDF.selectExpr("count_if(email IS NULL)")
usersDF.where(col("email").isNull()).count()
Question 5 of 20
A data engineer needs a reference to the results of a query that can be referenced across multiple queries within the scope of the environment session. The data engineer does not want the reference to exist outside of the scope of the environment session.

 

Which of the following approaches accomplishes this without explicitly dropping the data object? Select one response. 

 


They can store the results of their query within a table.

They can store the results of their query within a common table expression (CTE).

They can store the results of their query within a view.

They can store the results of their query within a reusable user-defined function (UDF).

They can store the results of their query within a temporary view.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
They can store the results of their query within a temporary view.
Question 6 of 20
A data engineer has a table high_temps with the following schema, where avg_high_temp represents the monthly average high temperatures for each unique year-month combination. 

 

year string

month string

avg_high_temp string

 

They need to reformat the data with years as the primary record key and months as the columns. The existing average high temperature value for each year-month combination needs to be in the month columns. 

 

How can the data engineer accomplish this? Select one response.

 


The data engineer can rotate the data from long to wide format using the .groupBy()clause.

The data engineer can rotate the data from long to wide format using the .pivot() function.

The data engineer can rotate the data from long to wide format using the .transform()clause.

The data engineer can rotate the data from wide to long format using the .pivot() function.

The data engineer can rotate the data from wide to long format using the .transform() clause.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The data engineer can rotate the data from long to wide format using the .pivot() function.
Question 7 of 20
A data engineer has the following query, where path is a variable that represents the location of a directory.

 

Query:

SELECT * FROM csv.`${path}`;

 


The query displays the underlying file contents of a directory of CSV files.

The query streams data from a directory of CSV files into a table.

The query loads the contents of a directory of CSV files from a source table to a target table.

The query displays the metadata of a directory of CSV files.

The query converts a directory of files into CSV format.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The query displays the underlying file contents of a directory of CSV files.
Question 8 of 20
A data engineer has a DataFrame events_df that has been registered against an external JSON file.  They need to access the field date within events_df. The events_df DataFrame has the following schema:

 

date string

month string

event_type string

 

Which of the following approaches can the data engineer use to accomplish this? Select one response.    

 


They can index the query by subfield using events[date] syntax.

They can use from_json() to parse the column for date.

They can use : syntax to access date in events_df.

They can use date.* to pull out the subfields of events_df into their own columns.

They can use . syntax to access date in events_df.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
They can use : syntax to access date in events_df.
Question 9 of 20
A data engineer needs to query a JSON file whose location is represented by the variable path.

 

Which of the following commands do they need to use? Select one response.

 


DISPLAY TABLE json.`${path}`;

SELECT * FROM path LOCATION `${path}`;

SELECT * FROM json.`${path}`;

RETURN json.`${path}`;

SHOW TABLE json.`${path}`;
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
SELECT * FROM json.`${path}`;
Question 10 of 20
Which of the following commands returns a new DataFrame from the DataFrame usersDF without duplicates? Select one response.


usersDF.distinct()

usersDF.select(*)

usersDF.drop()

usersDF.groupBy(nulls)

usersDF.count().dropna()
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
usersDF.distinct()
Question 11 of 20
A data engineer has created a DataFrame exploded_eventsDF created from the table exploded_events defined here:

 

CREATE TABLE events (user_id string, event_name string, item_id string, events struct<coupon:string, event_id:string, event_revenue:double>);

 

They are using the following code with multiple array transformations to return a new DataFrame that shows the unique collection of the columns event_name and items. 

 

Code block:

 

from pyspark.sql.functions import array_distinct, collect_set, flatten

 

exploded_eventsDF

    .groupby("user_id")

    .agg(collect_set("event_name"),

     _____

 

Which of the following lines of code fills in the blank to create the column event_history as a unique collection of events? Select one response.

 


array_distinct(flatten(collect_set("events.event_id"))).alias("event_history")

flatten(extract(events.event_id)).alias("event_history")

flatten(collect_set(explode(events:event_id))).alias("event_history")

array_distinct(extract(collect_set(events.event_id))).alias("event_history")

flatten(array_distinct(events[event_id])).alias("event_history")
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
array_distinct(flatten(collect_set("events.event_id"))).alias("event_history")
Question 12 of 20
A data engineer has a DataFrame with string column email_address. They are using a regular expression that returns a string with a matching pattern when it is in the following format: 

user.address@domain.com

Which of the following lines of code creates a new column domain that contains the domain from the email_address column? Select one response. 

 


.withColumn("domain", regexp_extract("email_address", "(?<=@).+", 0))

.withColumn("domain", array_distinct("email_address", "(?<=@).+", 0))

.withColumn("domain", collect_set("email_address", "(?<=@).+", 0))

.withColumn("domain", flatten("email_address", "(?<=@).+", 0))
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
.withColumn("domain", regexp_extract("email_address", "(?<=@).+", 0))
Question 13 of 20
A data engineer is using the following code block to create and register a function that returns the first letter of the string email. Another data engineer points out that there is a more efficient way to do this.

Code block:

from pyspark.sql.functions import udf

 

@udf("string")

def first_letter_function(email: str) -> str:

    return email[0]

 

first_letter_udf = spark.udf.register("sql_udf", first_letter_function)

 

Which of the following identifies how the data engineer can eliminate redundancies in the code? Select one response. 

 


They can eliminate the parameters in the function declaration.

They can eliminate "sql_udf" from the statement that registers the function.

They can eliminate the statement that registers the function.

They can eliminate the return statement at the end of the function.

They can eliminate the import statement in the beginning of the code block.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
They can eliminate the statement that registers the function.
Question 14 of 20
A data engineer has a table records with a column email. They want to check if there are null values in the email column. 

 

Which of the following approaches accomplishes this? Select one response.

 


They can check if there is at least one record where email is null by creating a data expectation to drop null values.

They can check if there is at least one record where email is null by pivoting the table on null values.

They can check if there is at least one record where email is null by running a regular expression function on email to filter out null values.

They can check if there is at least one record where email is null by adding a filter for when email IS NULL to a SELECT statement.

They can check if there is at least one record where email is null using SELECT DISTINCT records.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
They can check if there is at least one record where email is null by adding a filter for when email IS NULL to a SELECT statement.
Question 15 of 20
A data engineer needs to extract the calendar date and time in human readable format from a DataFrame containing the timestamp column user_last_touch_timestamp.


Which of the following lines of code correctly fills in the blank by adding the column end_date of type date in human readable format? Select one response.


.withColumn(date_time("end_date"), user_last_touch_timestamp, "MMM d, yyyy")

.withColumn(date_format("end_date"), user_last_touch_timestamp, "HH:mm:ss")

.withColumn(date_time("end_date"),user_last_touch_timestamp, "HH:mm:ss")

.withColumn("end_date", date_format("user_last_touch_timestamp", "MMM d, yyyy"))

.withColumn("end_date", CAST(user_last_touch_timestamp) as date_format)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
.withColumn("end_date", date_format("user_last_touch_timestamp", "MMM d, yyyy"))
Question 16 of 20
A data engineer wants to extract lines as raw strings from a text file. 

 

Which of the following SQL commands accomplishes this task? Select one response.

 


SELECT * FROM `${dbfs:/mnt/datasets}/001.txt`;

SELECT (*) FROM ${dbfs:/mnt/datasets}/001.txt`;

SELECT text(*) FROM ${dbfs:/mnt/datasets}/001.txt`;

SELECT * FROM `${dbfs:/mnt/datasets}/001.txt` as TEXT;

SELECT * FROM text.`${dbfs:/mnt/datasets}/001.txt`;
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
SELECT * FROM text.`${dbfs:/mnt/datasets}/001.txt`;
Question 17 of 20
A data engineer has a query that directly updates the files underlying the external table emails. 

 

Which of the following correctly describes how to retrieve the number of rows in the updated table? Select one response.

 


REFRESH TABLE emails;
SELECT COUNT(*) FROM emails;

REFRESH TABLE emails;
SELECT DISTINCT_COUNT(*) FROM emails;

REFRESH TABLE emails;
SELECT COUNT(*) FROM emails AS OF VERSION 1;

REFRESH TABLE emails;
SELECT DISTINCT_COUNT(*) FROM emails AS OF VERSION 1;

REFRESH TABLE emails;
SELECT COUNT(*) FROM emails WHEN UPDATED = TRUE;
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
REFRESH TABLE emails;
SELECT COUNT(*) FROM emails;
Question 18 of 20
A data engineer has a DataFrame events_df that has been registered against an external JSON file. The nested JSON fields have already been converted into struct types. The data engineer now needs to flatten the struct fields back into individual columns for the field event_type. The events_df DataFrame has the following schema:

 

date string

month string

event_type StructType<id string, size int>

 

Which of the following approaches allows the data engineer to retrieve id within event_type? Select one response.    

 


They can use from_json() to parse the columns for id.

They can use event_type.* to pull out id into its own column.

They can use : syntax to access id in event_type.

They can index the DataFrame by id.

They can use . syntax to access id in event_type.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
They can use . syntax to access id in event_type.
Question 19 of 20
A data engineer has created the following Spark DataFrame sales_df that joins the previously created table sales with the Spark DataFrame items_df when sales and items_df have matching values in the sales_id column in both data objects. 

 

Code block:

 

sales_df = (spark

    .table("sales")

    .withColumn("item", explode("items"))

)

 

items_df = spark.table("item_lookup")

 

item_purchasesDF = (sales_df

   ______________________)

 

Which of the following lines of code correctly fills in the blank? Select one response.

 


.merge(items_df, sales_df, on = "item_id")

.join(items_df, sales.sales_id == items_df.sales_id, how = "cross")

.join(items_df, sales_df.sales_id == items_df.sales_id)

.innerJoin(items_df, sales_df.sales_id == items_df.sales_id)

.outerJoin(items_df, sales.sales_id == items_df.sales_id)
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
.join(items_df, sales_df.sales_id == items_df.sales_id)
Question 20 of 20
A data engineer is registering a table in Databricks using the table users from an external SQL database. One of their colleagues gives them the following code to register the table. However, when the data engineer runs the code, they notice an error.

 

Code block:

CREATE TABLE users_jdbc
USING JDBC
OPTIONS (
  url = "jdbc:sqlite:${DA.paths.ecommerce_db}"
)

 

Which of the following correctly identifies why running the code is resulting in an error? Select one response.


The line dbtable = "users" needs to be added to OPTIONS.

USING JDBC needs to be changed to USING SQL.

A username and password need to be added to OPTIONS.

CREATE TABLE needs to be changed to CREATE JDBC TABLE.

None of these responses correctly identify the cause of the error.
Options
You gave the correct answer and your score is 10
Correct answer
Score: 10
Correct answer:
The line dbtable = "users" needs to be added to OPTIONS.