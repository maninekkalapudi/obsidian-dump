Our (Data Engineer’s) job is to marshal an organization’s data and bring it to business users for their decision making.

  

### Chapter 1- Data Warehousing, Business Intelligence, and Dimensional Modeling Primer

  

- **Different Worlds of Data Capture and Data Analysis**
    
    One of the most important assets of any organization is its information. This asset is almost always used for two purposes: operational record keeping and analytical decision making.
    
    Simply speaking, the operational systems are where you put the data in, and the DW/BI system is where you get the data out.
    
    Users of an operational system turn the wheels of the organization. The operational systems are optimized to process transactions quickly. Given this execution focus, operational systems typically do not maintain history, but rather update data to reflect the most current state.
    
    Users of a DW/BI system, on the other hand, watch the wheels of the organization turn to evaluate performance. DW/BI users almost never deal with one transaction at a time.
    

  

- **Goals of Data Warehousing and Business Intelligence**
    
    The pain points of BI/DW and business management people
    
    - “We collect tons of data, but we can’t access it.”
    - “We need to slice and dice the data every which way.”
    - “Business people need to get at the data easily.”
    - “Just show me what is important.”
    - “We spend entire meetings arguing about who has the right numbers rather than making decisions.”
    - “We want people to use information to support more fact-based decision making.”
    
    Now turn these business management quotations into requirements:
    
    - _**The DW/BI system must make information easily accessible.**_
        - The data must be intuitive and obvious to the business user, not merely the developer.
        - Business users want to separate and combine analytic data in endless combinations.
        - The business intelligence tools and applications that access the data must be simple and easy to use.
    
      
    
    - _**The DW/BI system must present information consistently.**_
        - The data in the DW/BI system must be credible. Data must be carefully assembled from a variety of sources, cleansed, quality assured, and released only when it is fit for user consumption.
        - Consistency also implies common labels and definitions for the DW/BI system’s contents are used across data sources.
    
      
    
    - _**The DW/BI system must adapt to change.**_
        - User needs, business conditions, data, and technology are all subject to change. The DW/BI system must be designed to handle this inevitable change gracefully so that it doesn’t invalidate existing data or applications.
        - If descriptive data in the DW/BI system must be modified, you must appropriately account for the changes and make these changes transparent to the users.
    
      
    
    - _**The DW/BI system must present information in a timely way.**_
        - As the DW/BI system is used more intensively for operational decisions, raw data may need to be converted into actionable information within hours, minutes, or even seconds. The DW/BI team and business users need to have realistic expectations for what it means to deliver data when there is little time to clean or validate it.
    
      
    
    - _**The DW/BI system must be a secure bastion that protects the information assets.**_
        - An organization’s informational crown jewels are stored in the data warehouse.
        - The DW/BI system must effectively control access to the organization’s confidential information.
    
      
    
    - _**The DW/BI system must serve as the authoritative and trustworthy foundation for improved decision making.**_
        - The data warehouse must have the right data to support decision making. The most important outputs from a DW/BI system are the decisions that are made based on the analytic evidence presented; these decisions deliver the business impact and value attributable to the DW/BI system.
        - a decision support system
    - _**The business community must accept the DW/BI system to deem it successful.**_
        - Unlike an operational system implementation where business users have no choice but to use the new system, DW/BI usage is sometimes optional. Business users will embrace the DW/BI system if it is the “simple and fast” source for actionable information.
    
    With a DW/BI initiative, you have one foot in your information technology (IT) comfort zone while your other foot is on the unfamiliar turf of business users.
    
      
    
    DW/BI manager responsibilities:
    
    - _**Understand the business users:**_
        - Understand their job responsibilities, goals, and objectives.
        - Determine the decisions that the business users want to make with the help of the DW/BI system.
        - Identify the “best” users who make effective, high-impact decisions.
        - Find potential new users and make them aware of the DW/BI system’s capabilities.
    
      
    
    - _**Deliver high-quality, relevant, and accessible information and analytics to the business users**_:
        - Choose the most robust, actionable data to present in the DW/BI system, carefully selected from the vast universe of possible data sources in your organization.
        - Make the user interfaces and applications simple and template-driven, explicitly matched to the users’ cognitive processing profiles.
        - Make sure the data is accurate and can be trusted, labeling it consistently across the enterprise.
        - Continuously monitor the accuracy of the data and analyses.
        - Adapt to changing user profiles, requirements, and business priorities, along with the availability of new data sources.
    - Sustain the DW/BI environment:
        - Take a portion of the credit for the business decisions made using the DW/BI system and use these successes to justify staffing and ongoing expenditures.
        - Update the DW/BI system on a regular basis.
        - Maintain the business users’ trust.
        - Keep the business users, executive sponsors, and IT management happy.
    
      
    
    **Dimensional Modeling Introduction**
    
    Dimensional Modeling is a technique to present analytic data with the requirements:
    
    - Deliver data that’s understandable to the business users.
    - Deliver fast query performance.
    
    Simplicity is critical because it ensures that users can easily understand the data, as well as allows software to navigate and deliver results quickly and efficiently.
    
    The ability to visualize something as abstract as a set of data in a concrete and tangible way is the secret of understandability.
    
    Although dimensional models are often instantiated in relational database management systems, they are quite different from third normal form (3NF) models which seek to remove data redundancies.
    
    Normalized 3NF structures divide data into many discrete entities, each of which becomes a relational table.
    
    Entity-relationship diagrams (ER diagrams or ERDs) are drawings that communicate the relationships between tables.
    
    the key difference between 3NF and dimensional models is the degree of normalization.
    
    Normalized 3NF structures are immensely useful in operational processing because an update or insert transaction touches the database in only one place.
    
    Normalized models, however, are too complicated for BI queries. Most relational database management systems can’t efficiently query a normalized model; the complexity of users’ unpredictable queries overwhelms the database optimizers, resulting in disastrous query performance.
    
      
    
    _**Star Schemas Versus OLAP Cubes**_
    
    Dimensional models implemented in relational database management systems are referred to as star schemas because of their resemblance to a star-like structure.
    
    Dimensional models implemented in multidimensional database environments are referred to as online analytical processing (OLAP) cubes,
    
    ![[Learning and Creation/Attachments/Untitled.png|Untitled.png]]
    
    Rererence: [What is OLAP? | IBM](https://www.ibm.com/topics/olap)
    
    When data is loaded into an OLAP cube, it is stored and indexed using formats and techniques that are designed for dimensional data.
    
    Performance aggregations or precalculated summary tables are often created and managed by the OLAP cube engine.
    
    Consequently, cubes deliver superior query performance because of the precalculations, indexing strategies, and other optimizations. Business users can drill down or up by adding or removing attributes from their analyses with excellent performance without issuing new queries.
    
      
    
    _**Fact Tables for Measurements**_
    
    - The term fact represents a business measure. Fact table in a dimensional model stores the performance measurements resulting from an organization’s business process events.
        
        ![[Learning and Creation/Attachments/Untitled 1.png|Untitled 1.png]]
        
    - Each row in a fact table corresponds to a measurement event. The data on each row is at a specific level of detail, referred to as the grain.
    - Store the low-level measurement data resulting from a business process in a single dimensional model. Because measurement data is overwhelmingly the largest set of data, it should not be replicated in multiple places for multiple organizational functions around the enterprise.
    - One of the core tenets of dimensional modeling is that all the measurement rows in a fact table must be at the same grain.
        
        Ex: Order details table. The data from each order which has products sold, quantity, unit price and final price for each order.
        
    - Facts are sometimes semi-additive or even non-additive.
    - Semi-additive facts, such as account balances, cannot be summed across the time dimension.
    - Non-additive facts, such as unit prices, can never be added.
    - Fact table grains fall into one of three categories: transaction, periodic snapshot, and accumulating snapshot.
    - All fact tables have two or more foreign keys that connect to the dimension tables’ primary keys. You access the fact table via the dimension tables joined to it.
    - Fact table generally has its own primary key composed of a subset of the foreign keys. This key is often called a _**composite key**_.
    - Every table that has a composite key is a fact table. Fact tables express many-to-many relationships. All others are dimension tables.
    
      
    
    _**Dimension Tables for Descriptive Context**_
    
    - Dimension tables are integral companions to a fact table. The dimension tables contain the textual context associated with a business process measurement event. They describe the “who, what, where, when, how, and why” associated with the event.
        
        ![[Learning and Creation/Attachments/Untitled 2.png|Untitled 2.png]]
        
    - Dimension attributes serve as the primary source of query constraints, groupings, and report labels.
    - It is sometimes unclear whether a numeric data element is a fact or dimension attribute. You often make the decision by asking whether the column is a measurement that takes on lots of values and participates in calculations (making it a fact) or is a discretely valued description that is more or less constant and participates in constraints and row labels (making it a dimensional attribute).