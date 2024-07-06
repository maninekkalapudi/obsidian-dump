---
Start Date: 2022-11-10
End Date: 2022-11-12
Status: Done
Assigned: MManikantha Nekkalapudi
---
# Intro

Hello all! Hope you are doing good.

In the last couple of years, you might’ve heard a lot about Data Engineering. It surely gained a lot of buzz in recent times and… every company wanted data engineers. Needless to say, the demand for data engineers was at an all-time high.

But what is data engineering though? And why do we need it? Let's understand all of that in this post with the following topics:

1. What is Data Engineering? And why do we need it?
2. Responsibilities of a Data Engineering Team
3. Challenges

# What is Data Engineering? And why do we need it?

Simply put, data engineering deals with collecting, storing, processing the data in a data warehouse and serving that data to various stakeholders.

Data will be generated in a company by different teams at variety of systems like databases, APIs, streaming events, file servers etc. This is the data required by different teams to carry out various analysis.

Generally, the incoming data is in different formats and sizes from different sources and that data is stored into an archival/analytics system like a data warehouse or a data lake. When the data is in data warehouse, it will be cleaned, transformed into a mutually agreed format between the stakeholders.

The data engineering team will build and maintain the pipelines and processes like ETL/ELT for data ingestion and data transformations across all the data that is being received in the data warehouse.

The end goal of a data engineering efforts is analytics ready data or “clean data”.

  

_**Why a centralized system like data warehouse?**_

To ensure that all the company’s data is in one single system and any team looking for particular data can easily access it. This ensures that there is no overhead for any team to obtain the required data in a common format that is used across the entire organization

This also means that there is no duplication of effort across any teams for creating any dataset from multiple sources.

# Responsibilities of a Data Engineering Team

- Identify data sources, analyze the data and ingest the data into the data warehouse
- Build and maintain the data pipelines for periodic ingestion and processing of the data
- Adding resiliency to the pipelines for failures
- Build and maintain the data warehouse tables and specialized datasets
- Maintaining data quality and integrity
- Last but not least, maintain and scale the data infrastructure

# Challenges

- The whole data engineering effort is internal to a company mostly. There is no customer interaction, nor any direct revenue generated here. There will be a number of questions on its viability, credibility and ROI
- There can be a lot of incoming data requests from various teams across entire company
- [Context switching](https://maximebeauchemin.medium.com/the-downfall-of-the-data-engineer-5bfb701e5d6b). The data engineering team handles a good number of pipelines, and they will be taking up further tasks collaborating with different teams to fulfill their data requests. Handling all of these things at once will require context switching and that might affect the quality in the long run.

  

You can find more details, examples on the data engineering and few important questions about it in the below video

[https://youtu.be/dEDc25k7Kck](https://youtu.be/dEDc25k7Kck)

# Resources

- [The Harsh Reality of Being a Data Engineer - YouTube](https://www.youtube.com/watch?v=VdR2WxQNnwg)
- [Why Are Data Teams Still Struggling to Answer Basic Questions - YouTube](https://www.youtube.com/watch?v=goT7gN1lwBI)
- [Bloomberg Doesn't Understand My Job (As An Ex-Meta Data Engineer) - Triggered Data Guy - YouTube](https://www.youtube.com/watch?v=4NXzeZYaZqQ)