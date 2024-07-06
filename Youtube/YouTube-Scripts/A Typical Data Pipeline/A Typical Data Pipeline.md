# **Introduction**

Hello people, Hope y’all doing well. This is Mani. Welcome to channel!

As data engineers, we build data pipelines to collect data from different source systems and place it in an analytics system i.e., a data warehouse/data lake. Ex: database records, web events and etc.

  

_**What is a data warehouse?**_

A data warehouse is centralized data storage/analytical system for say, a company which enables them to run complex queries on large scale data.

  

_**In this video, we will talk about**_

1. What is a data pipeline? Example of a typical data pipeline
2. What is ETL and ELT?
3. What is a data warehouse, data lake?
4. Why do we need a data pipeline?
5. Technical skills that are used at each level

  

## 1. _**What is a data pipeline? Example of a typical data pipeline**_

We know that a data pipeline collects data from different source systems and moves it to the analytics system. Lets improvise that definition a bit

_**A data pipeline is a series of interconnected systems that passes data in only one direction, i.e., from source to serving layer with increasing order of clarity and value in the data**_

  

Let me clarify this a bit more…

The data received from a source can contain duplicate records, test records and records that are problematic.

The pipeline should be designed in such a way that it eliminates all these issues and only then the data is moved from raw to staging to serving thus increasing the clarity and value in it

  

Now, we will discuss about the stages in the data pipeline from the example on the screen

1. _**Data Sources**_ - Where data is generated, recorded or obtained.
    
    - Ex: database, SFTP file server, an API and etc.
    - The data in different systems can be generated at different intervals and all of it needs to land safely in our DWH
    
    _**Note:**_ In practice, similar data sources might be grouped into one
    
      
    
2. _**Raw layer**_ - First level of data storage on the DWH. This is an archival storage layer and the data stored in it will not be modified at any time
    
    - A raw layer will have a designated location in our warehouse for every data source
    
      
    
3. _**Staging/Transform layer**_ - Second level of data storage in the DWH.
    
    - The data in this layer is cleaned, transformed in to a certain format and then stored in a staging table
    - Staging layer also unifies the data from multiple sources in different formats into a single format that is required internally or requested by the end users
        - Ex: Payment confirmation from payment gateway is in JSON and transaction record is in a database table
    - Typically, a staging layer will try to retain most of the information in the form of tables as much as possible without any duplicates records or any problematic records
    
      
    
4. _**Serving layer**_- This is the aggregated data layer and the data will be sourced from different tables in the staging layer
    
    - Typically, this can be a simple view that is fetching data from different staging tables
    - Serving layer will provide the aggregated data to BI dashboards, Business reporting, ML workflows and Data sharing with APIs
    - This layer will retain only the aggregated data mostly
    
      
    

![[Learning and Creation/Attachments/Untitled 3.png|Untitled 3.png]]

  

_**Note:**_ The raw layer should never be modified at any given point. It should stay as the data source for further processes happening in the pipeline.

## **2. What is ETL and ELT**

- E-Extract, T-Transform, L-Load
- ETL/ELT is a process to get the data into an analytical system like a DWH or Data lake preferably on a schedule
    - Each data pipeline will have a schedule time to run and not overburden the data infrastructure at certain times in a day
- In ETL, the data is extracted from a source, transformed into final format and loaded into DWH table
    - This is well suited when the data is in database and needs minimal changes
- In ELT, the data is extracted from a source, loaded into the raw layer. When the transformations are defined later in the future, the data is transformed and loaded into the final tables in DWH
    - This is well suited when the data from different sources are coming in and the transformations for the data are not defined completely. The data is transformed when they are available and the subsequent staging table is modified accordingly
- If we are bringing the data for the first time, the data will be loaded, then analysis will be carried on that data before we actually build the rest of the pipeline. Ex: data from an API in JSON format

## **3. What is a data warehouse and data lake?**

- **Data warehouse**- Centralized data repository that stores all the historical and the current data from various sources. This data can be used for analytics—→ ETL
- **Data lake**- A data lake is also a centralized data repository which stores structured, semi-structured and unstructured data——→ ELT

## **4. Why do we need a data pipeline?**

- **Explain an example about an ecommerce company with a huge volume of transactions**
    - _Large volumes of data_ coming in from _different sources_(Apps, web events, transactions, images/videos, telemetry data)
    - _Different types_, and sizes of data
    - The above characteristics i.e., Volume, Variety, Velocity are attributed to “Big Data”
- Centralized data repository for an entire organization
    - Data is not siloed and anyone with right access can always access the data and perform the analysis
- **Storing the data and using the it in downstream analytics**
    - Storing the data as is in our storage(Raw layer)
    - Cleaning and transforming the data
- **Data quality and resiliency**
    - Ex: Restoring the data from data loss/corruption
- **Need for distributed compute**
    - Performing the large scale in-depth analytics
    - Data and the analytics systems should be accessible across a large org consistently

![[Learning and Creation/Attachments/Untitled 1.png]]

  

![[Learning and Creation/Attachments/Untitled 2 2.png|Untitled 2 2.png]]

- Suppose, business team needs daily sales reports. So, an ELT job should run on a daily schedule based on when the report is needed and also in which shape and form

## 5. **Technical skills that are used at each level**

The list of skills are listed based on non-GUI tools.

- One programming language - Python preferably
- SQL - The lingua franca of data world
- Big data processing systems - Hadoop, Spark
- Linux
- Cloud optional

![[Learning and Creation/Attachments/Untitled 3 2.png|Untitled 3 2.png]]