---
Start Date: 2020-09-30
End Date: 2020-09-26
Status: Published
Assigned: MManikantha Nekkalapudi
---
Hello! Hope you're doing well. Welcome to an intro on Data Engineering. This post will be a 10,000 feet overview of the field with an example of Spotify. So, Let's get started!

Topics covered in this post are:

1. Introduction to Big Data
2. OLTP and OLAP Systems
3. Extract, Transform and Load (ETL)
4. Distributed Systems in Data Engineering
5. Hadoop Ecosystem
6. Conclusion

## 1. Introduction to Big Data Processing

[Spotify](https://www.spotify.com/), one of the well-known names in the music streaming industry. At the time of writing this post it has a collection of over [50 Million songs](https://www.businessofapps.com/data/spotify-statistics/#:~:text=Source%3A%20Goodwater%20Capital-,Spotify%20Content%20Statistics,doing%20a%20lot%20a%20legwork.) and [700,000+ podcasts](https://techcrunch.com/2020/04/29/spotifys-catalog-tops-a-million-podcasts-consumption-increased-by-triple-digits-over-last-year/#:~:text=The%20company%20says%20it%20has,was%20reporting%20just%20this%20March.). Well, that is not an easy number to store, process and serve 286 million monthly active Spotify users.

Now, having a lot of data doesn't really mean anything unless we derive the insights from it. Valuable insights would eventually generate excellent value to the listening experience, which is particularly important for Spotify's business. We will see with the below examples.

  

Spotify curates it's music streaming experience to all its users based on their tastes in music and podcasts. This adds a huge value by giving customers a consistent music listening experience. The following image shows a music personalization feature called "Discover Weekly".

![[Spotify.jpg]]

Image source: [Spotify — Product #4 by Keerthi Abinesh Ravikumar](https://medium.com/@keerthiabinesh/spotify-product-4-acd614ebf052)

Another such example I really love is [Spotify Wrapped](https://engineering.atspotify.com/2020/02/18/spotify-unwrapped-how-we-brought-you-a-decade-of-data/#:~:text=The%20Spotify%20Wrapped%20Campaign%20is,social%20campaigns%20of%20the%20year.&text=Because%20Wrapped%20is%20such%20a,data%2C%20frontend%20and%20backend%20engineering.). A Wrapped is Spotify's largest marketing and social campaigns of the year providing users to see a detailed breakdown of their listening habits over the past year. In 2019, Spotify wanted to provide users with a review over the entire decade (from 2010 to 2019).

  

For this massive engineering feat, they had to crunch ~5X more data than every year for 248 million monthly active users. This was the largest [Dataflow](https://cloud.google.com/dataflow) (Data Processing service) jobs to ever run on the Google Cloud Platform in 2019. Top Artists, Top Songs, and Top Podcasts are some of the stats it provided to every user.

  

![[Untitled 24.png|Untitled 24.png]]

The architecture of data pipelines for 2019 Wrapped

  

The smooth orchestration of such activities related to storage and processing massive data while maintaining the quality, integrity and durability of the data is called as Data Engineering. This is a gross over-simplified explanation, and it is enough for now.

## 2. OLTP and OLAP Systems

The entry point to understanding Data Engineering or Big Data is understanding the following systems:

1. Online Transaction Processing System (OLTP) - Deals with current data. At max, a week's data
2. Online Analytical Processing System (OLAP) - Deals with historical data.

  

OLTP systems are best suited to handle

- Latest or day-to-day data
- capable of processing large number of transactions (insert, update and delete data in database tables)
- Data in the form of rows in a database table
- Where the latency of applications is extremely low. Any application like Ecommerce purchases, Bank transactions etc.

Relational databases like MySQL, Oracle, MSSQL Server are very well suited for this type of task

  

OLAP systems on the other hand are best suited for:

- Storing huge amount of historical data in the form of files
- Processing huge data for business insights. Here we're not bothered about the speed of data retrieval and some OLAP systems doesn't even provide a way to delete/update single records
- Purely for Data Analysis, reporting etc. Ex: Targeted sale campaigns on Ecommerce site, Monthly, yearly sales estimates for each department in the organization.

Data Warehouses like Teradata, Cloudera's EDH (Enterprise Data Hub), AWS S3 are well suited for this type of tasks.

  

Well, why do we need two different systems for data processing? OLTP systems are very transaction heavy systems and are busy in serving user's requests. They store current data which would be a week or two worth of data at max (depending on the resources, no. users this might vary significantly). Later, the data should be offloaded to a Data Warehouse. Getting any kind of insights from the data in these systems will not be possible.

  

OLAP systems on the other hand can store and process massive amounts of historical data for insights. For generating the business intelligence, visualization, Machine Learning etc. these systems are extensively used. In Data Engineering, we will discuss about OLAP systems for the most part.

  

This type of two stage setup is quite common in modern data pipelines. This offers a good amount of flexibility and quite easy to troubleshoot errors independent of each other.

![[Untitled 1 10.png|Untitled 1 10.png]]

Source: [https://www.guru99.com/oltp-vs-olap.html](https://www.guru99.com/oltp-vs-olap.html)

### 3. Extract, Transform and Load (ETL)

ETL, one of the most common terms used in Data Engineering. The following are the stages in it:

1. Extract- Extracts data from sources like RDBMS, File servers, Mobile/Web apps etc. and stores it in staging area
2. Transform- Transforms the data from one form to another. Ex: Removing spaces in string columns, currency conversion for the prices etc. in the staging area
3. Load- Load the data to the tables in the Data Warehouse

  

The following diagram shows the ETL process with different stages:

![[Untitled 2 10.png|Untitled 2 10.png]]

We often encounter another term called "Pipeline" or "ETL Pipeline" in data engineering. A pipeline is a set of tools + processes which moves raw data through different stages in a singular direction while converting it into a desired form at each stage. In simple terms, the goal of a pipeline is to extract the useful data from different data source and load it to a Data Warehouse.

  

Once a pipeline is setup, it can be triggered as per business requirement to ingest the data from data sources to the data warehouse.

### 4. Distributed Systems in Data Engineering

Storage and processing of huge data using ETL pipelines requires a cluster of machines working in parallel and in coordination. This is not an easy feat by any means. In fact, Google had to come up with a completely new programming paradigm called “[MapReduce](https://en.wikipedia.org/wiki/MapReduce)" to process massive data using a cluster of machines. This went on to become a watershed moment in big data processing.

  

Later, A new file system for storing massive data called “[HDFS](https://en.m.wikipedia.org/wiki/Apache_Hadoop#HDFS)” was designed. In HDFS, every file is split into blocks (each of 128MB) and multiple copies of each blocked is stored on a cluster of machines. A master of this cluster called “Namenode” keeps track of all the blocks and their health, availability.

  

In traditional programming, the code and data should reside on the same machine to work. With MapReduce, the code is sent to the machines where data resides, and results are calculated there in parallel unlike the traditional methods. Later the results are calculated will be written to disk. The details and location of the blocks(files) are obtained from Namenode in HDFS.

  

In our above example, Spotify should be using something like HDFS for large file storage (audio files, images, Database dumps etc.) and MapReduce for processing massive data to compute the metrics for Wrapped 2019. Again, this is just for the sake of an argument and it'll vary in practice.

### 5. Hadoop Ecosystem

The Hadoop ecosystem is versatile and offers a wide variety of tools for Data Ingestion, Distributed File storage, Batch Processing, Machine Learning and much more. The below picture lists some of the tools.

![[Untitled 3 10.png|Untitled 3 10.png]]

Source: [https://data-flair.training/blogs/hadoop-ecosystem-components/](https://data-flair.training/blogs/hadoop-ecosystem-components/)

- HDFS- Distributed File System
- MapReduce- A Big Data processing engine
- YARN (Yet Another Resource Negotiator)- Resource Manager for scheduling jobs
- Oozie- Workflow manager
- Hive- Facilitates writing MapReduce job with SQL Syntax
- HBase- A distributed database on top of HDFS
- Sqoop- Tool to ingest data from Relational databases to HDFS
- Zookeeper- Keeps sync of all the running services in a distributed system
- and many more...

The above listed are the frequently used components in Hadoop ecosystem and are used at various points in analytics pipelines (includes data engineering pipeline as well).

### 6. Conclusion

The evolution of large scale data and data intensive applications gave rise to plethora of Data Engineering tools.

In recent years MapReduce is being replaced by [Apache Spark](https://spark.apache.org/) with it's much better processing engine and in-memory computing. But the principles and internals of MapReduce are still valid in Spark as well. We will explore more about those in the upcoming posts.

  

Thank you

Mani