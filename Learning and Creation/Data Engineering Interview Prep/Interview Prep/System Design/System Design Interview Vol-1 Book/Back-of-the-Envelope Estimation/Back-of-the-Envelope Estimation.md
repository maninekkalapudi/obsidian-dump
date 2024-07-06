  

Back-of-the-envelope calculations are estimates you create using a combination of thought experiments and common performance numbers to get a good feel for which designs will meet your requirements - Jeff Dean

  

### Data Volume Units:

![[Untitled 47.png|Untitled 47.png]]

### Latency numbers every programmer should know

![[Untitled 1 20.png|Untitled 1 20.png]]

  

### Visualized latency Numbers (as of 2020)

![[Untitled 2 19.png|Untitled 2 19.png]]

**Conclusions:**

- Memory is fast but the disk is slow.
- Avoid disk seeks if possible.
- Simple compression algorithms are fast.
- Compress data before sending it over the internet if possible.
- Data centers are usually in different regions, and it takes time to send data between them.

### Availability numbers

High availability is the ability of a system to be continuously operational for a desirably long period of time.

![[Untitled 3 16.png|Untitled 3 16.png]]

A service level agreement (SLA) is a commonly used term for service providers. This is an agreement between you (the service provider) and your customer, and this agreement formally defines the level of uptime your service will deliver.

Cloud providers Amazon, Google and Microsoft set their SLAs at 99.9% or above. Uptime is traditionally measured in nines. The more the nines, the better.

  

### Example: Estimate Twitter QPS and storage requirements

**Assumptions:**

- 300 million monthly active users
- 50% of users use Twitter daily
- Users post 2 tweets per day on average
- 10% of tweets contain media
- Data is stored for 5 years  
      
    

**Estimations:**

Query per second (QPS) estimate:

- Daily active users (DAU) = 300 million * 50% = 150 million
- Tweets QPS = 150 million * 2 tweets / 24 hour / 3600 seconds = ~3500
- Peek QPS = 2 * QPS = ~7000

  

**We will only estimate media storage here.**

- Average tweet size:
    - tweet_id 64 bytes
    - text 140 bytes
    - media 1 MB
- Media storage: 150 million * 2 * 10% * 1 MB = 30 TB per day
- 5-year media storage: 30 TB * 365 * 5 = ~55 PB

  

**Note:** Please note the above numbers are for this exercise only as they are not real numbers from Twitter.