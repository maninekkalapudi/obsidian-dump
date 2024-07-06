## Prelude

In the modern data engineering scene, storage is cheaper, and compute has become powerful. The data processing has transitioned from ETL (Extract, Transform, Load) to ELT (Extract, Load, Transform).

The ELT processing gives the ultimate flexibility in transforming the data whenever needed. Later in the process, the results of these transformations and the integrity of the resultant data should also be tested to ensure trust.

What if we could somehow automate the testing process for the data? Ensuring data quality and trust in the data is of paramount importance. This is where _[dbt](https://www.getdbt.com/)_ comes into picture.

## What is dbt?

dbt, short for _**data build tool**_; helps transform the data in the warehouse by writing “**SELECT”** statements. dbt handles turning these select statements into tables and views.

Simply put, the role of dbt is to handle only the “Transformation” part in the ELT process. The extract process will be handled by the data ingestion tools.

![[Untitled 45.png|Untitled 45.png]]

dbt also handles the testing part as well. We can specify the generic or custom test cases for every field in a dataset. Once the testing is done, it can be deployed into the production in a warehouse through a version-controlled environment.

Each dataset will be developed, tested in a version controlled environment and the latest changes can be promoted to production along with its test cases. When the dataset runs in production, the data will be loaded after it goes through those test cases

![[Untitled 1 18.png|Untitled 1 18.png]]

## Typical dbt project

  

  

  

  

  

  

  

![[Untitled 2 17.png|Untitled 2 17.png]]

  

  

  

  

  

  

  

  

## References

1. [What is dbt? | dbt Docs (getdbt.com)](https://docs.getdbt.com/docs/introduction#what-else-can-dbt-do)
2. [What, exactly, is dbt? (getdbt.com)](https://www.getdbt.com/blog/what-exactly-is-dbt/)
3. [Positioning data build tool, dbt, in the data tooling landscape - Solita Data](https://data.solita.fi/positioning-data-build-tool-dbt-in-the-data-tooling-landscape/)