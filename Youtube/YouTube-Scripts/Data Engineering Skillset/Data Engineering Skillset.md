Hello All, Hope you’re doing good and welcome back to another video

First things first, Thanks for the 100 subs! Means a lot. More content is coming and you know what to do.

<Add subscribe button here>

Before we jump on to the video, I recommend you to go through the “What is Data engineering?” and “A typical data pipeline” videos (links in the description below) to understand this one better.

Now, lets jump into the video!

# Data Engineering skillset

- As data engineers we deal with a lots of data. We build a lot of processes using code to work with the data. That means we deal with a bit of Software Engineering aspects as well
- Essentially, “Data Engineering” is “Data” + “Software Engineering”
- Now we can understand the skills required for both the categories

![[Learning and Creation/Attachments/Untitled 2.png|Untitled 2.png]]

### SQL

- SQL or Structured Query Language is the language of the relational databases, which was widely used format to query the relational data. Ex: Orders data, customer data and etc.
- Nowadays SQL is even more popular in DE world too. It can be used for tasks like data exploration, cleaning, transforming, perform in-depth analysis and generating datasets etc.

### A Programing language (Python)

- Python is a programming/scripting language that is also ubiquitous in data world. DE, AI, ML, data mining and so on uses python and libraries written in it on a daily basis
- In DE, the data is sourced from different systems like databases, file servers, APIs and so on. We need to connect to all those systems and bring in the data into our warehouse
- Python can used to almost all the data sources and retrieve the data into data warehouse. Data ingestion can be scheduled as a timely activity using python using scheduling tools like Airflow
- Once the data is in the warehouse, we can use python to clean the data, run some checks on it and even transform the data as well
- Python can be used in automation

### Data Warehouse

- Since the data is very huge and growing, it cannot be stored on a single machine
- Distributed computing architecture like Hadoop provides a simpler way to store and process data by dividing the data and the compute tasks among multiple nodes/computers connected via network
- Every modern data warehouse takes advantage of the Distributed computing
- Having distributed cluster means we can scale the resources to run pipelines on a schedule, run some ad-hoc analysis, testing data quality and etc. within a single cluster
- As DEs, we need to understand how the data is stored on say, a hadoop cluster or cloud storages and how it is processed by compute framework like Apache Spark
- Spark is a widely used data processing engine which has python wrapper to it called PySpark. We can use python or even SQL to specify the tasks that we wanted to perform on the data.

  

### Additional skillset

- Pipeline Orchestration tool like Airflow, Azure Data Factory and etc.
- Cloud services relevant to data engineering
- Linux cli
- Git

  

[https://twitter.com/adi_12_modi/status/1581220185327144960?s=20&t=bsFnAJyHvSFrdjl4otrKeg](https://twitter.com/adi_12_modi/status/1581220185327144960?s=20&t=bsFnAJyHvSFrdjl4otrKeg)