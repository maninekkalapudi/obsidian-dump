---
Start Date: 2021-08-17
End Date: 2021-09-05
Status: Started
Assigned: MManikantha Nekkalapudi
---
Hello! Hope you're doing well. We have discussed about [MapReduce](https://maninekkalapudi.com/hadoop-mapreduce) with [wordcount example](https://maninekkalapudi.com/wordcount-example-with-mapreduce) and [HDFS](https://maninekkalapudi.com/hdfs-hadoop-distributed-filesystem) in the previous posts. But, have you ever wondered how do we get the data to the HDFS in the first place? We will discuss about Sqoop which is a tool to bulk ingest the data from a database to HDFS. Let's dive in!

Topics covered in this post:

1. What is Sqoop?
2. Running Ad-hoc SQL Commands on a Database With Sqoop
3. Sqoop Import Command
4. Sqoop Import Examples
5. Split Size and Boundary Query
6. Sqoop Import Execution Flow
7. Compression Techniques
8. Understanding Sqoop split-by
9. Sqoop Export
10. Sqoop Job
11. Sqoop Incremental

### 1. What is Sqoop?

Sqoop is a tool designed to transfer the data between a database and HDFS. It bulk loads the data from a database like MySQL, Oracle and etc., into HDFS in the form of files. The database however stores the same data in the form of records in the database.

It helps us automate the data transfer between a database and HDFS. Once the data is imported in to the HDFS cluster, the transformations are applied on the data using MapReduce programs.

Once we obtain the results from MapReduce, the data can be loaded back to the database for visualizations, reporting and etc.

![[Untitled 26.png|Untitled 26.png]]

  

Sqoop has a CLI (Command Line Interface) to run the commands to import and export the data.

  

**Note****:** Here, I'm using the Cloudera Hadoop VM on Windows machine for the demo. You can find the installation guide [here](https://www.youtube.com/watch?v=HP4g2BU7-xU). Once it's done, open a terminal and type `sqoop help` to check the Sqoop version and the available commands

![[Untitled 1 12.png|Untitled 1 12.png]]

  

### 2. Running Ad-hoc SQL Commands on a Database With Sqoop

Sqoop offers a way to run the SQL commands like list databases, tables and any ad-hoc queries against a database or a table. This will help us to have a look at the data before running a Sqoop job or building a data ingestion pipeline. First, we'll see how that is done with mysql cli and later with Sqoop.

1. Connect to the mysql database with the below command:

```Shell
mysql -u root -p # login command for mysql database
```

- `-u` - username for the mysql database. For the mysql database in cloudera VM, username is 'root'
- `-p` - password for the database. Password is 'cloudera'

b. After logging in to the database, view the list of databases with the following command:

```Shell
show databases; # command to show list of databases
```

![[Untitled 2 12.png|Untitled 2 12.png]]

  

c. Select the `retail_db` database from the list of databases and display the data in the `categories` table using the following commands:

```Shell
use retail_db; \#select the retail_db database
show tables; \#displays all the tables
select * from categories limit 10; # Display first 10 rows from categories table
```

![[Untitled 3 12.png|Untitled 3 12.png]]

  

Here, we've logged into the mysql database and checked the tables for the data. We can check the same with the Sqoop commands without logging into mysql.

1. **List all databases**
    
    `sqoop-list-databases` command will list all the databases in the database
    

```Shell
sqoop-list-databases --connect jdbc:mysql://<db-hostname>:<db-port> \
--username <username> --password <password>

Ex:
sqoop-list-databases --connect jdbc:mysql://quickstart.cloudera:3306 \
--username retail_dba --password cloudera
```

- `--connect` - jdbc connection string to connect to the mysql database. jdbc connection is one of the most used method to connect to any type of database programmatically
- `--username`, `--password` - username and password to login to the database

  

**Output:**

![[sqoop_list_all_databases.png]]

  

b. **List all tables**

`sqoop-list-tables` command will list all the tables in a database

```Shell
sqoop-list-tables --connect jdbc:mysql://<db-hostname>:<db-port>/<db-name> \
--username <username> --password <password>

Ex:
sqoop-list-tables --connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba --password cloudera
```

Here, we need to specify the database name in the connection string itself. Ex: "jdbc:mysql://quickstart.cloudera:3306/**_retail_db_**"

**Output****:**

![[sqoop_list_all_tables.png]]

  

c. **sqoop-eval command**

`sqoop-eval` command will let us run any adhoc queries against any table in any database. This will be very helpful in understanding the data before we start importing it.

```Shell
sqoop-eval --connect jdbc:mysql://<db-hostname>:<db-port> \
--username <username> --password <password> \
--query "<sql-query>"

sqoop-eval --connect jdbc:mysql://quickstart.cloudera:3306 \
--username retail_dba --password cloudera \
--query "select * from retail_db.customers limit 10"
```

- `--query` - the ad-hoc SQL query

**Output****:**

![[sqoop_eval.png]]

### 3. Sqoop Import Command

`sqoop-import` command is an important and widely used command among the Sqoop commands and it does the following:

- imports the data from a database table to HDFS
- treats each row in a table as a record in HDFS
- allows to store the records as text file, Sequence file, Avro and Parquet file formats

  

How does the `sqoop-import` command work internally?

- Internally, All Sqoop commands are MapReduce jobs and `sqoop-import` job is a special type of MapReduce job, where only mappers would work and no need of reducers because no aggregation is required. For a detailed blog post on Map Reduce, follow [this link](https://maninekkalapudi.com/hadoop-mapreduce)
- By default, 4 mappers will do the data importing in parallel and this can be changed
- Mappers divide the work based on the primary key (PK) of the table which is being imported. The PK values must be numbers to split the data. If the PK values are strings, then we need to use a special parameter to split the data.
- If there is no PK in the table, Sqoop can't divide the work between mappers. In this case
    - Change the number of mappers to 1. This is fine for small datasets
    - For large datasets with no PK, We can use `split-by` on a column to divide the data among the mappers

The default behavior of Sqoop is to import the data parallelly using 4 mappers. To achieve that, Sqoop needs to split the data into equal number of chunks for each mapper with the help of primary key of the table. If only one mapper is doing most of the work, then the job would take a very long time to complete and thus affecting the parallelism.

### 4. Sqoop Import Examples

Lets dive deep into how `sqoop-import` command works with example but first lets have a look on the data in the mysql table. Here we're inspecting the data from the `orders` table in `retail_db`.

  

1. Login to the mysql database as mentioned in the above section. After logging in, query the data from the `orders` table in `retail_db`. Here, mysql database is the source and the HDFS location is the target system.

```SQL
use retail_db; --change database to retail_db
describe orders; --shows the column names and their data types, constraints
select * from orders limit 50; --show first 50 records 
select count(*) from orders; --count of records in the orders table
```

Here, we can see the orders table having the columns such as `order_id`, `order_date`, `order_customer_id`, `order_status`and the primary key for the `orders` table is `order_id`.

![[Untitled 4 10.png|Untitled 4 10.png]]

One more thing we can observe is that, the `order_id` column is set to auto increment i.e. every new`order_id` will be increased by 1. This is really important and it affects the performance of the `sqoop-import` command. We will discuss more on this later.

b. Now, we can run the following `sqoop-import` command to import the data from orders table in to HDFS path (`/sqoop-import-orders`)

```Shell
sqoop-import --connect jdbc:mysql://<db-hostname>:<db-port>/<db-name> \
--username <username> --password <password> \
--table <table-name> --target-dir <hdfs-dir>

Ex:
sqoop-import --connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba --password cloudera --table orders \
--target-dir /sqoop-import-orders
```

The following are the different parameters used in `sqoop import` command:

- `--connect`- jdbc connection string with database name
- `--table`- table to be imported. This will pick up all the records present in the table at the time of importing.
- `--target-dir`- This is an HDFS location and it shouldn't be an existing directory.

By default, the number of mappers are 4. So, 4 files will be created in the `targer-dir`. Lets run the command and see the target HDFS path. The imported data will be in the default text file format. You can use the following commands to check the output in the `--target-dir`

```Shell
hadoop fs -ls <target-dir> # lists the files in the --target-dir

hadoop fs -cat <target-dir>/<file-name> | wc -l # getting the record count from the output file
```

Output:

![[Untitled 5 9.png|Untitled 5 9.png]]

  

As mentioned earlier, we have 4 files in the `--target-dir` HDFS path. Since only mappers ran internally, the output of each mapper is stored in the form of files and each filename has the naming `part-m-000x` ("m" represents mapper).

In the above picture, each file has the record count of 17221 or 17720. How does each file get that count? We will see this in the next section.

### 5. Boundary Query and Split Size

We understood that the data in `--target-dir` HDFS path is present in the form of 4 files and we have seen that each file has records of either 17221 or 17220. To understand how we ended up with this number, we must observe the `sqoop-import` output.

The following stages will be executed in the `sqoop-import` command:

1. BoundingValQuery or Boundary Query
2. Split size
3. MapReduce job URL
4. Map job

![[Untitled 6 9.png|Untitled 6 9.png]]

![[Untitled 7 8.png|Untitled 7 8.png]]

  

- **BoundingValQuery or Boundary Query**
    
    The `sqoop-import` command will divide the work among the mappers (default 4 mappers) based on the primary key (pk). In the `orders` table, `order_id` is the primary key. The boundary query will get the `min(pk)` and `max(pk)` for the table at the time of import using the query below:
    
    ```SQL
    SELECT MIN(order_id), MAX(order_id) FROM orders;
    
    -- For Orders table:
    -- MIN(order_id)=1, MAX(order_id) = 68883
    ```
    
- **Split Size**
    
    Once we obtain the `min(pk)` and `max(pk)` from Boundary Query, the split size is calculated using the formula below:
    

```Markdown
split_size = (max(pk) - min(pk))/ num_mappers

For Orders table:
split_size = (68883 - 1)/4

split_size = 17220.5 -> Either the mapper can have 17220 or 17221 records to import
```

Each mapper will be processing the records in the range mentioned below:

**Mapper1 → 1 to 17,221 (17221)**

**Mapper2 → 17,222 to 34,442 (17221)**

**Mapper3 → 34,443 to 51,662 (17220)**

**Mapper 4 → 51,663 to 68,883 (17221)**

Now that we understood how the split size is calculated from pk values, we will see how the pk values affect the performance of the `sqoop-import` job further in the later section.

  

- **MapReduce job URL and Map job**
    
    Every sqoop command or job is registered in the YARN (Resource Manager) as MapReduce job for tracking purpose. You can see the job being registered in the YARN URL (`[http://localhost:8088/<job_id>](http://localhost:8088/%3Cjob_id%3E)`) in the browser.
    

### 6. Primary Key Values and Sqoop Import performance

Now that we understand how a `sqoop-import` job internally splits the data among the mappers. We need to understand the implications on the job performance when the Primary Key values are skewed.

Lets understand this with the bad record (unusual `order_id` which introduces skew in the pk values) example on the `orders` table. The `orders` table has total of 68883 records in the source table and the `order_id` is increasing by 1 as observed earlier. Now, lets add a bad record to `orders` table with the `order_id` "1000000" table.

  

![[Untitled 8 6.png|Untitled 8 6.png]]

  

After adding the bad record, now lets run the same `sqoop-import` command again and check how the job performs.

![[Untitled 9 5.png|Untitled 9 5.png]]

The `split-size` is calculated with the `min(pk)` and `max(pk)` from the Boundary Query. Since we added the skewed record (i.e. `order_id`= 1000000), this will be considered as the `max(pk)`. Accordingly, the `split-size` (249999) is calculated.

Most of the records (apart from the bad record) will fall under the interval of 1-249999 range and it is processed by the mapper1 and rest of the mappers will not process any records because of the skewed `order_id` value.

You can observe the same in the below image. All the old records will be processed by mapper1 and only the new record is processed by the mapper4.

![[Untitled 10 4.png|Untitled 10 4.png]]

In this example, we understood how a bad or skewed records will impact the parallelization of the `sqoop-import` job. There is a way we could improve this type of situation and we will have a look into it in the later section

_**Note:**_

While running the `sqoop-import` command, if the `--target-dir` is existent then Sqoop will throw the "**Output directory <hdfs-location> already exists**" error. This is to avoid any overwrites to the existing data in the HDFS.

![[Untitled 11 4.png|Untitled 11 4.png]]

### 5. Sqoop Import Execution Flow

The `sqoop-import` job undergoes the following steps to ingest the data

1. Get metadata (schema) of the table by running a simple query. `SELECT t.* from '<table-name>' AS t LIMIT 1`
2. Build a POJO (Plain Old Java Object) with appropriate getters and setters. The datatypes of the columns in a database table will be mapped to Java datatypes. Learn about POJO [here](https://www.javatpoint.com/pojo-in-java)
3. Compile the POJO class into a JAR file
4. Run boundary query to get `min(pk)` and `max(pk)` on the Primary Key column or on the `split-by` column
5. Compute the split-size using the formula and compute the split intervals for each mapper
6. Submit MapReduce job with number of mappers as 4 (default)
7. Each map task will run a `SELECT` statement on the source table with where condition based on the split intervals to read the data
8. Data will be written to the files in the location specified with `--targret-dir` or `--warehouse-dir`

![[Untitled 12 4.png|Untitled 12 4.png]]

  

  

```Markdown
sqoop-import --connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba -P --table orders --target-dir /sqoop-import-orders-seq \
--as-sequencefile
```

  

  

1. `sqoop export` command
    - To export the data (in set of files) from the HDFS to database
    - The target table must exist in the database already
    - Files are read and parsed into a set of records according to the user-specified delimiters