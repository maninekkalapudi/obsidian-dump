---
Start Date: 2022-10-28
End Date: 2022-11-01
Status: Done
Assigned: MManikantha Nekkalapudi
---
# **Introduction**

Hello people, Hope you are doing well.

As data engineers, we build data pipelines to collect data from different source systems and place it in an analytics system i.e., a data warehouse/data lake. The data is usually sourced systems like a database, web events, or an API and etc.

  

_**In this post, we will talk about**_

1. What is a data pipeline?
2. Stages of a data pipeline
3. What is ETL and ELT?
4. What is difference between data warehouse and data lake?
5. Why do we need a data pipeline?

  

## 1. _**What is a data pipeline?**_

We know that a data pipeline collects data from different source systems and moves it to the analytics system. Let's improvise that definition a bit

_**A data pipeline is a series of interconnected systems that passes data in only one direction, i.e., from source to serving layer with increasing order of clarity and value in the data**_

The data received from a source may contain duplicate records, test records and records that are problematic. Data pipeline should be designed in such a way that it eliminates all these issues and only then the data is moved from raw to staging to serving thus increasing the clarity and value in it

## 2. Stages of a data pipeline

A typical data pipeline will have below stages (refer above picture).

1. Sources
2. Raw
3. Staging
4. Serving

The below picture shows the different stages of a data pipeline

![[Untitled 43.png|Untitled 43.png]]

  

1. _**Data Sources**_: Where data is generated, recorded or obtained for the data pipeline. For example, a database, an SFTP file server, an API and etc.
    
    The data in different systems can be generated at different intervals and all of it needs to land safely in our data warehouse. The schedule to extract the data will be decided by the data engineering team.
    
    _**Note**_: In practice, similar data sources might be grouped into one pipeline while collecting the data.
    
      
    
2. _**Raw layer**_: It is the first level of data storage on the data warehouse. This is an archival storage layer and the data stored in it will not be modified at any time.
    
    Raw layer will have a designated location for every data source in our warehouse like a directory. The data in it should never be modified at any given point. It should stay as the data source for further processes happening in the pipeline.
    
      
    
3. _**Staging/Transform layer**_ - It is the second level of data storage in the data warehouse. The data in this layer, which is sourced from raw layer, is cleaned, transformed into a certain format and then stored in various staging tables.
    
    Typically, a staging layer will try to retain most of the information from the raw layer without any duplicate records or problematic records.
    
    Staging layer also unifies the data from multiple sources in different formats into a single format. This unified data might be requested by the end users.
    
    For example, the order data like payment confirmation from the payment gateway (json data) and the transaction record (database table) is requested to be in a single tale by end users. In the transformation step, the data from these two sources will be merged into one single staging table.
    
      
    
4. _**Serving layer**_: This is the aggregated data layer, and the data will be sourced from different staging tables. For example, average amount spent by the customers over the years
    
    Typically, this can be a simple view or a separate table that is fetching data from different staging tables. Serving layer will provide the aggregated data to BI dashboards, Business reporting, ML workflows and data sharing with APIs for example.
    

## 3**. What is ETL and ELT**

ETL stands for Extract, Transform and Load. ETL/ELT is a process to get the data into an analytical system like a data warehouse or data lake, preferably on a schedule.

  

_**ETL**_

In ETL, the data is extracted from a source, transformed into final format and loaded into DWH table. This is well suited when the data is in a database where the data needs minimal changes.

  

_**ELT**_

In ELT, the data is extracted from a source, loaded into the raw layer. When the transformations are defined in the future, the data is transformed readily and loaded into the final tables in the data warehouse.

ELT is well suited when the data comes from different sources and the transformations for all the data are not defined completely. The data is transformed when they are available, and the subsequent staging table is modified accordingly.

## 4**. What is difference between data warehouse and data lake?**

|   |   |
|---|---|
|**Data Warehouse**|**Data Lake**|
|Stores processed data|Stores raw data|
|Transformations are fully defined|Transformations are not fully defined|
|Stores structured data in tabular format in a data warehouse tables|Stores the structured, semi-structured and unstructured data in raw form|
|More complicated and costly to make changes to the tables|Highly accessible and quick to update|
|File formats like parquet, ORC, delta and etc. are used|File formats like CSV, text files, parquet files, PDFs etc. are used|

## **4. Why do we need a data pipeline?**

Large volumes of data coming in from different sources(Apps, web events, transactions, images/videos, telemetry data). These data is of different types, and sizes

These characteristics i.e., volume, variety, velocity are attributed to “Big Data”. To process the big data in a consistent manner while serving the data to an entire organization is a huge challenge. Data pipelines are built by data engineers to solve this problem.

Once the data is processed, it is stored in a centralized data repository like data warehouse. This ensures that the data is not siloed and anyone with right access can always access the data and perform the analysis.

Data pipelines can also ensure the data quality at scale. Any checks that needs to be performed on the data can be applied on the transformation stage ensuring quality.

Since we also maintain the raw data, the pipelines will be made resilient of failures, any data loss or corruption can be eliminated using them by reprocessing the existing data.

![[Untitled 1 16.png|Untitled 1 16.png]]

  

Here is a video on the same topic:

[https://youtu.be/0TYxyAqPZto](https://youtu.be/0TYxyAqPZto)

## Resources

1. [Data Lake vs Data Warehouse: Key Differences | Talend](https://www.talend.com/resources/data-lake-vs-data-warehouse/#:~:text=Data%20lakes%20and%20data%20warehouses,processed%20for%20a%20specific%20purpose.)
2. [Databases Vs. Data Warehouses Vs. Data Lakes | MongoDB](https://www.mongodb.com/databases/data-lake-vs-data-warehouse-vs-database)