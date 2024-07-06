# Intro

Hello all! Hope you are doing good. This is Mani and I’m back with another video

- In the last couple of years, you might’ve heard about Data engineering. It surely gained a lot of buzz and… every company wanted data engineers
- Needless to say, the demand for data engineers was at an all-time high
- **But what the hell is data engineering though?**
- **Why do we need it?**
- **Is that a completely new trend that emerged out of the sky all of a sudden?**
- ~~**Do we really need to know about it?**~~

  

Well, we have a video ahead of us to understand that and without further ado… let’s get started!

# Example

- Suppose a data analytics team in a company is tasked to generate a few reports for the business teams. Those reports will be used to make strategic decisions on the business side
- These reports might require data that belongs to different teams within the same company
- For example: the Finance and Sales teams have SAP, and the Marketing team has Salesforce CRM systems
- The analytics team might have to work with those data/product owners to obtain the required data. It can be a hassle if the data is sensitive
- When the data is obtained, chances are that the data is not standardized across these systems within the same company. Chances are that the same entity can be represented differently in different systems.
- The goal of the analytics team is to send the reports to the stakeholders at a scheduled time using the acquired data
- Since the data received is not in the same format, the analytics team will have to clean, standardize and process the data to generate the required output.
- Let's assume that the reports generated are for the quarter
- The requirements might change in the future and the data needs to be acquired again
- All of these tasks are clearly an overhead to the analytics team and might impact on their delivery timelines
- Also, the data quality and accuracy will be in constant scrutiny from the stakeholders
- In this scenario, there is a lot of “engineering” that needs to be done in order to obtain the data and serve the reports
- Fun fact, neither the data analytics team nor the product teams want to own these efforts as they have their own goals to deliver
- So, who will maintain these processes? How to ensure the data quality consistently? Data Engineers!

# What is Data Engineering? And why do we need it?

- Simply put, data engineering deals with collecting, storing, processing and serving very large amounts of data
- Incoming data is in different formats and sizes from different sources into an archival/analytics system like a data warehouse
- When the data is in data warehouse, it will be cleaned, transformed into a mutually agreed format between the stakeholders
- The data engineering team will build pipelines and processes like ETL for data ingestion and data transformations
- Data Pipeline video in the description below
- The end goal of a data engineering pipelines is analytics ready data or “clean data”.
- _**Why a centralized system like data warehouse?**_
- To ensure that all the company’s data is in one single system and any team looking for a particular data can easily access that data
- This ensures that there is no overhead to obtain the required data by a team in a common format that is used across the organization
- No duplication of effort across any teams for accessing required data

# Is DE a completely new trend?

- NO!
- All of these activities in the data engineering role might sound familiar and that is true.
- The Data industry, just like any other field in IT, has seen some significant transformations over the years
- We had on-prem infra for the majority of the data system like databases and data warehouses where database engineers, data warehouse specialists, data integration teams and ETL engineers worked
- Later Hadoop came into the spotlight, which used multi-node cluster environments
- We had Big Data engineers and Hadoop admins which used a combination of Hadoop ecosystem tools to work in that environment and later with Apache Spark on Hadoop
- Now, the clouds appear everywhere, roles like Data Warehouse engineers, ETL developers, Big Data developers are consolidated into a single discipline called “Data Engineering”
- All of these roles chase one true dream called “Clean Data”, which they still do this day
- To finally answer the question… NO! DE is not a trend. It is the core of every org which does analytics

# Responsibilities of a Data Engineering Team

- Identify data sources, analyze the data and ingest the data into the data warehouse
- Build and maintain the data pipelines for periodic ingestion and processing of the data
- Adding resiliency to the pipelines for failures
- Build and maintain the data warehouse tables and datasets
- Create datasets for different stakeholders
- Maintaining data quality and integrity
- Last but not least, maintain and scale the data infrastructure
- In short, all the tasks that the product or analytics teams doesn’t want to do to get the clean data 😉

# Challenges

- The whole data engineering effort is internal to a company mostly. There will be a number of questions on its viability, credibility and ROI
- Incoming data requests on the data teams
- Context switching. Link to the article [https://maximebeauchemin.medium.com/the-downfall-of-the-data-engineer-5bfb701e5d6b](https://maximebeauchemin.medium.com/the-downfall-of-the-data-engineer-5bfb701e5d6b)
- [The Harsh Reality of Being a Data Engineer - YouTube](https://www.youtube.com/watch?v=VdR2WxQNnwg)
- [Why Are Data Teams Still Struggling to Answer Basic Questions - YouTube](https://www.youtube.com/watch?v=goT7gN1lwBI)

  

- [Bloomberg Doesn't Understand My Job (As An Ex-Meta Data Engineer) - Triggered Data Guy - YouTube](https://www.youtube.com/watch?v=4NXzeZYaZqQ)