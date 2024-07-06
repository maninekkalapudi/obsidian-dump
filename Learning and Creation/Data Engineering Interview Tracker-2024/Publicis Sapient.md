---
Contact: Chetan Shriramwar<mailto:chetan.shriramwar@publicissapient.com>
Interview Date: Invalid date
Interview Source:
  - Naukri
Remote / Location: Hyderabad
Source: Naukri
Status: on hold
Tier: Low Tier
Tracking: Email
---
### Written Test

1. PS Data Engineering Coding Test Sr Associate - was cancelled. Contacted the recruiter over the phone but no response
    1. Completed the written test. One Python question with list operations
        
        5
        
        Amir,21,Indore
        
        Jack,28,Kerala
        
        Amir,21,Bangalore
        
        Hari,29,Hyderabad
        
        Chenna,23,Chennai
        
        Find uniquie records based on Name and Age
        
          
        
2. PS Data Engineering MCQ Test Sr Associate- 6 questions. completed in under 15 mins
    
    - **How does the example spark scala code executes? The example was with a function and that function had a colect() statement**. colect() statement always sends the result data to the driver. select that option
    - **Does spark streaming has save as hadoopfile function?** I’ve selected 3 as the answer
        - **3 tables with 1 unique join column:** This will trigger **1 MapReduce job**. All the data from the involved tables gets shuffled based on the chosen join column, processed in the reduce phase, and finally joined.
        - **3 tables with 2 unique join columns:** This will trigger **2 MapReduce jobs**, one for each join column. This means the data gets shuffled and processed based on each join column separately, followed by an additional round of shuffling and joining in the final reducer.
        - **3 tables with 3 unique join columns:** This will trigger **3 MapReduce jobs**, one for each join column. The process follows the same pattern as before, with separate shuffles, processing, and joining for each column.
    - Spark streaming write function. Pick odd one out
        - saveashadoopfile (no such thing as hadoopfile)
        - `**foreachRDD**`
        - `**foreachBatch**`
        - `**writeStream**`
    
    ### Technical Interview
    
    - Call Scheduled on 20-02-2024 at 12pm IST
    - Project discussion and tech stack discussion
    - Comments on why Databricks and Synapse are used in the stack
    - ADF- How to connect to on-prem server?
        - **Self-Hosted Integration Runtime (SHIR):**
            - This option installs a lightweight agent on your on-premises machine that acts as a bridge between ADF and your server.
            - Advantages:
                - No need for VPN or firewall adjustments.
                - Suitable for hybrid data integration scenarios.
            - Disadvantages:
                - Requires installation and configuration on the on-premises machine.
                - Might have performance limitations depending on network setup.

- SCD Type 2 implementation in Spark for the below problem
    
    - PK1 PK2 VAL1 VAL2 VAL3 , IS_ACTIVE, CreationDate, TerminationDate
        
        First Load  
        K1, K2, A,B,C  
        
        Second Load  
        K1, K2, A,B,C  
        K1, K3, A,N,C  
        
        3rd Load  
        K1, K2, A,B,X  
        K1, K3, A,N,C  
        
        PK1,PK2,VAL1,VAL2,VAL3, IS_ACTIVE, CreationDate, TerminationDate  
        K1, K2, A,B,C 0 ,  
        
        K1, K2, A,B,X 1  
        K1, K3, A,N,C 1  
        
        **Answer:(wrong)**
        
        MERGE INTO target_table a T  
        USING source_table as S  
        ON T.PK1 = S.PK1 AND T.PK1 = S.PK1  
        WHEN MATCHED THEN UPDATE  
        T.VAL1 = S.VAL1,  
        T.VAL2 = S.VAL2,  
        T.VAL3 = S.VAL3,  
        T.IS_ACTIVE = 1,  
        T.CreationDate = S.CreationDate,  
        T.TerminationDate = current_date()  
        WHEN NOT MATCHED THEN INSERT *  
        
        **Correct Answer:**
        
        MERGE INTO target_table USING source_table AS s  
        ON T.PK1 = S.PK1 AND T.PK1 = S.PK1  
        
        WHEN MATCHED AND target_table.end_date IS NULL THEN  
        UPDATE SET  
        T.TerminationDate = current_date,  
        
        T.IS_ACTIVE = 0
        
        WHEN MATCHED AND target_table.end_date IS NOT NULL THEN  
        INSERT (PK1, PK2,VAL1,VAL2,VAL3, IS_ACTIVE, CreationDate, TerminationDate)  
        VALUES (s.PK1, s.PK2, s.VAL1, s.VAL2, s.VAL3, s.IS_ACTIVE, s.CreationDate, NULL)  
        
        WHEN NOT MATCHED THEN  
        INSERT (PK1, PK2,VAL1,VAL2,VAL3, IS_ACTIVE, CreationDate, TerminationDate)  
        VALUES (s.PK1, s.PK2, s.VAL1, s.VAL2, s.VAL3, s.IS_ACTIVE, s.CreationDate, NULL)  
        
    
      
    
- EMP csv columns: EmpId, EName, DeptId, Salary
    
    - Read the CSV file
    - filter the records for Second highest Salary for each department
    - write to delta table
    - Sample Data:
        - for dept d1, salaries  
            100  
            100  
            100  
            95  
            90  
            
    
      
    
    **Answer:**
    
    emp_df = spark.read.format('csv').load('<csv_path>', header=True)  
    emp_df.createOrReplaceTempView('emp_df')  
    
      
    
    sec_highest = spark.sql('select *, dense_rank() over(partition by DeptId order by Salary desc) as sal_num qualify sal_num = 2')
    
      
    
    **Note:** Practice PySpark code and use PySpark APIs in the solutions for interviews