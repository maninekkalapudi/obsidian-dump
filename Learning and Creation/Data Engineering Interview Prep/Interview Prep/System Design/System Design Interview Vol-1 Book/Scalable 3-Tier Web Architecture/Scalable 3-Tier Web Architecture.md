## A typical single server setup

The architecture of a simple website or an app, for example a blog or a shopping portal; for the user to visit would look something like this:

![[Untitled 46.png|Untitled 46.png]]

- Users can either visit the website or open an app using the domain name. For example, [google.com](http://google.com) or www.mysite.com
- Usually, the Domain Name System (DNS) is a paid service provided by 3rd parties and not hosted by our servers.
- Internet Protocol (IP) address is returned to the browser or mobile app. In the example,  
    IP address 15.125.23.214 is returned.  
    
- Once the IP address is obtained, [Hypertext Transfer Protocol (HTTP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) requests are  
    sent directly to your web server.  
    
- The web server returns HTML pages or JSON response for rendering.

  

_**Web application:**_ it uses a combination of server-side languages (Java, Python, etc.) to  
handle business logic, storage, etc., and client-side languages (HTML and JavaScript) for  
presentation  

  
  
_**Mobile application:**_ HTTP protocol is the communication protocol between the mobile  
app and the web server. JavaScript Object Notation (JSON) is commonly used API  
response format to transfer data due to its simplicity. An example of the API response in  
JSON format is shown below:  

GET /users/12 – Retrieve user object for id = 12

![[Untitled 1 19.png|Untitled 1 19.png]]

  

## Database

A database stores all the data related to the business. The database could technically be installed on a webserver but it is not advisable.

![[Untitled 2 18.png|Untitled 2 18.png]]

Separating web/mobile traffic (web tier) and database (data tier) servers allows them to be scaled independently. It also avoids any potential security issues as well.

_**What kind of a database should be used?**_

You can choose between a traditional relational database and a non-relational database. Let  
us examine their differences.  

  
Relational databases are also called a relational database management system (RDBMS) or  
SQL database. The most popular ones are MySQL, Oracle database, PostgreSQL, etc. Relational databases represent and store data in tables and rows. You can perform join  
operations using SQL across different database tables.  

  

Non-Relational databases are also called NoSQL databases. Popular ones are CouchDB,  
Neo4j, Cassandra, HBase, Amazon DynamoDB, etc. [2]. These databases are grouped into four categories: key-value stores, graph stores, column stores, and document stores. Join operations are generally not supported in non-relational databases  

  

**Non-relational databases might be the right choice if:**

• Your application requires super-low latency.  
• Your data are unstructured, or you do not have any relational data.  
• You only need to serialize and deserialize data (JSON, XML, YAML, etc.).  
• You need to store a massive amount of data.  

## Vertical scaling vs horizontal scaling

Vertical scaling→ “scale up” → adding more power (CPU, RAM, etc.) to your servers

_**Advantages:**_

When traffic is low, vertical scaling is a great option

**_Disadvantages:_**

- Vertical scaling has a hard limit. It is impossible to add unlimited CPU and memory to a  
    single server to achieve performance  
    
- Vertical scaling does not have failover and redundancy

  

Horizontal scaling → “scale-out” → scale by adding more servers into your pool of resources

_**Advantages:**_

- Horizontal scaling is more desirable for large scale applications

  

In the previous design, users are connected to the web server directly. Users will unable to  
access the website if the web server is offline.  

If many users access the web server simultaneously and it reaches the web server’s load limit, users generally experience slower response or fail to connect to the server. A load balancer is the best technique to address these problems.

## Load Balancer

A load balancer evenly distributes incoming traffic among web servers that are defined in a load-balanced set.

![[Untitled 3 15.png|Untitled 3 15.png]]

Users connect to the public IP of the load balancer directly to access the website. With this  
setup, web servers are unreachable directly by clients anymore for better security. Private  
IPs are used for communication between servers.  

A private IP is an IP address reachable only between servers in the same network; however, it is unreachable over the internet. The load balancer communicates with web servers through private IPs.

- If server 1 goes offline, all the traffic will be routed to server 2. This prevents the website  
    from going offline. A new healthy web server to the server pool to balance the load  
    
- If the website traffic grows rapidly, and two servers are not enough to handle the traffic,  
    the load balancer can add more servers to the web server pool, and automatically starts to send requests to them  
    

  

## Database replication

Database replication can be used in many database management systems, usually with a master/slave relationship between the original (master) and the copies (slaves)

A master database generally only supports write operations. A slave database gets copies of  
the data from the master database and only supports read operations  

All the data-modifying commands like insert, delete, or update must be sent to the master database. Most applications require a much higher ratio of reads to writes; thus, the number of slave databases in a system is usually larger than the number of master databases

![[Untitled 4 13.png|Untitled 4 13.png]]

_**Advantages of database replication:**_

- _**Better performance**_: All writes and updates happen in master nodes. Read operations are distributed across slave nodes. This model improves performance because it allows more queries to be processed in parallel
- _**Reliability**_: In case of a database failure, data is still preserved because data is replicated across multiple locations
- _**High availability:**_ By replicating data across different locations, your website remains in operation even if a database is offline as you can access data stored in another database  
    server.  
    

### Master and Slave node failures

- If only one slave database is available and it goes offline, read operations will be directed  
    to the master database temporarily. As soon as the issue is found, a new slave database  
    will replace the old one. In case multiple slave databases are available, read operations are  
    redirected to other healthy slave databases. A new database server will replace the old one.  
    
- If the master database goes offline, a slave database will be promoted to be the new  
    master. All the database operations will be temporarily executed on the new master  
    database. A new slave database will replace the old one for data replication immediately.  
    In production systems, promoting a new master is more complicated as the data in a slave  
    database might not be up to date. The missing data needs to be updated by running data  
    recovery scripts.  
    

![[Untitled 5 12.png|Untitled 5 12.png]]

  

## Cache

A cache is a temporary storage area that is much faster than the database and stores the result of expensive responses or frequently accessed data in memory. The subsequent requests for the same data are served more quickly.

The performance of a web app can greatly affected by calling the database repeatedly. Caching can mitigate this problem.

_**Advantages:**_

- Better system performance
- Ability to reduce database workloads
- Ability to scale the cache tier independently

### Read-through Cache

After receiving a request, a web server first checks if the cache has the available response. If  
it has, it sends data back to the client. If not, it queries the database, stores the response in  
cache, and sends it back to the client.  

![[Untitled 6 12.png|Untitled 6 12.png]]

Example for access cache for Memcached via API

![[Untitled 7 11.png|Untitled 7 11.png]]

  

_**Decide when to use cache:**_

when data is read frequently but modified infrequently. Since cached data is stored in volatile memory, a cache server is not ideal for persisting data. For instance, if a cache server restarts, all the data in memory is lost

  
  
_**Expiration policy:**_

It is a good practice to implement an expiration policy. Once cached data is expired, it is removed from the cache. When there is no expiration policy, cached data will be stored in the memory permanently.\

It is advisable not to make the expiration date too short as this will cause the system to reload data from the database too frequently. Meanwhile, it is advisable not to make the expiration date too long as the data can become stale.

  
  
_**Consistency:**_

This involves keeping the data store and the cache in sync. Inconsistency can happen because data-modifying operations on the data store and cache are not in a single transaction. When scaling across multiple regions, maintaining consistency between the data store and cache is challenging.  
  
  
_**Mitigating failures:**_

A single cache server represents a potential _**Single Point Of Failure (SPOF).**_ A SPOF is a part of a  
system that, if it fails, will stop the entire system from working. As a result, multiple cache servers across different data centers are recommended to avoid SPOF.  

Another recommended approach is to overprovision the required memory by certain percentages.  
This provides a buffer as the memory usage increases.  

![[Untitled 8 8.png|Untitled 8 8.png]]

_**Eviction Policy:**_

Once the cache is full, any requests to add items to the cache might cause existing items to be removed. This is called cache eviction.

Least-recently-used (LRU) and Least Frequently Used (LFU) or First in First Out (FIFO) are different eviction policies.

## Content delivery network (CDN)

A CDN is a network of geographically dispersed cache servers used to deliver static content like images, videos, CSS, JavaScript files, etc.

When a user visits a website, a CDN server closest to the user will deliver static content. Intuitively, the further users are from CDN servers, the slower the website loads

![[Untitled 9 7.png|Untitled 9 7.png]]

1. User A tries to get image.png by using an image URL. The URL’s domain is provided by the CDN provider.
    
    Image URLs look like on Amazon and Akamai CDNs:
    
    [https://mysite.cloudfront.net/logo.jpg](https://mysite.cloudfront.net/logo.jpg)
    
    [https://mysite.akamai.com/image-manager/img/logo.jpg](https://mysite.akamai.com/image-manager/img/logo.jpg)
    
2. If the CDN server does not have image.png in the cache, the CDN server requests the  
    file from the origin, which can be a web server or online storage like Amazon S3.  
    
3. The origin returns image.png to the CDN server, which includes optional [HTTP header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)  
      
    [Time-to-Live (TTL)](https://developer.mozilla.org/en-US/docs/Glossary/TTL) which describes how long the image is cached.
4. The CDN caches the image and returns it to User A. The image remains cached in the  
    CDN until the TTL expires.  
    
5. User B sends a request to get the same image, the image is returned from the cache as long as the TTL has not expired.

### Considerations of using a CDN

- _**Cost:**_ CDNs are run by third-party providers, and you are charged for data transfers in  
    and out of the CDN. Caching infrequently used assets provides no significant benefits so  
    you should consider moving them out of the CDN.  
    
- _**Setting an appropriate cache expiry:**_ For time-sensitive content, setting a cache expiry  
    time is important. The cache expiry time should neither be too long nor too short. If it is  
    too long, the content might no longer be fresh. If it is too short, it can cause repeat  
    reloading of content from origin servers to the CDN.  
    
- _**CDN fallback:**_ If there is a temporary CDN outage, clients should be able to detect the problem  
    and request resources from the origin.  
    - _**Invalidating files:**_ You can remove a file from the CDN before it expires by performing cache invalidation of the CDN object using CDN vendor APIs and use object versioning to serve a different version of the object.
    - For example, version number 2 is added to the query string: image.png?v=2.

![[Untitled 10 6.png|Untitled 10 6.png]]

  

### Stateless web tier

The user session data is typically stored on the webservers. A good practice is to store session data in the persistent storage (out of the web tier) such as relational database or NoSQL. Each web server in the cluster can access state data from databases.

### Stateful architecture

A stateful server remembers client data (state) from one request to the next and a stateless server does not store any data about the client.

![[Untitled 11 6.png|Untitled 11 6.png]]

User A’s session data and profile image are stored in Server 1. To authenticate User A, HTTP requests must be routed to Server 1. This request would fail if the authentication request is routed to any other server because those server does not contain User A’s session data

The issue is that every request from the same client must be routed to the same server. This can be done with sticky sessions in most load balancers

However, this adds the overhead. Adding or removing servers is much more difficult with this approach. It is also challenging to handle server failures

### Stateless architecture

HTTP requests from users can be sent to any web servers, which fetch state data from a shared data store. State data is stored in a shared data store and kept out of web servers. A stateless system is simpler, more robust, and scalable.

![[Untitled 12 6.png|Untitled 12 6.png]]

We move the session data out of the web tier and store them in the persistent data store. The shared data store could be a **relational database, Memcached/Redis, NoSQL**, etc.

The NoSQL data store is chosen as it is easy to scale. Autoscaling means adding or removing web servers automatically based on the traffic load

  

### Data centers

In normal operation, users are geoDNS-routed, also known as geo-routed, to the closest data center, with a split traffic of x% in US-East and (100 – x)% in US-West.

geoDNS is a DNS service that allows domain names to be resolved to IP addresses based on the location of a user.

![[Untitled 13 4.png|Untitled 13 4.png]]

  

_**Data Center Outage:**_

In the event of any significant data center outage, we direct all traffic to a healthy data center.  
In Figure 1-16, data center 2 (US-West) is offline, and 100% of the traffic is routed to data  
center 1 (US-East)  

![[Untitled 14 4.png|Untitled 14 4.png]]

Technical challenges to achieve multi-data center setup:

- _**Traffic redirection:**_ Effective tools are needed to direct traffic to the correct data center.  
    GeoDNS can be used to direct traffic to the nearest data center depending on where a user  
    is located.  
    
- _**Data synchronization:**_ Users from different regions could use different local databases or  
    caches. In failover cases, traffic might be routed to a data center where data is unavailable.  
    A common strategy is to replicate data across multiple data centers  
    
- _**Test and deployment:**_ With multi-data center setup, it is important to test your  
    website/application at different locations. Automated deployment tools are vital to keep  
    services consistent through all the data centers  
    

We should decouple the system's various components to allow for independent scaling. Many real-world distributed systems use a messaging queue as a key strategy to resolve solve this problem.

### Message queue

A message queue is a durable component, stored in memory, that supports asynchronous communication. It serves as a buffer and distributes asynchronous requests.

It serves as a buffer and distributes asynchronous requests. Input services, called producers/publishers, create messages, and publish them to a message queue.

Other services or servers, called consumers/subscribers, connect to the queue, and perform actions defined by the messages.

![[Untitled 15 4.png|Untitled 15 4.png]]

With the message queue, the producer can post a message to the queue when the consumer is unavailable to process it. The consumer can read messages from the queue even when the producer is unavailable.

### Logging, metrics, automation

_**Logging:**_ Monitoring error logs is important because it helps to identify errors and problems  
in the system. You can monitor error logs at per server level or use tools to aggregate them to  
a centralized service for easy search and viewing.  

  
  
_**Metrics:**_ Collecting different types of metrics help us to gain business insights and understand  
the health status of the system  

- Host level metrics: CPU, Memory, disk I/O, etc.
- Aggregated level metrics: for example, the performance of the entire database tier, cache  
    tier, etc.  
    
- Key business metrics: daily active users, retention, revenue, etc.

  

_**Automation:**_ When a system gets big and complex, we need to build or leverage automation  
tools to improve productivity. Continuous integration is a good practice,  

![[Untitled 16 4.png|Untitled 16 4.png]]

  

## Database Scaling-Vertical scaling

You can add more CPU, RAM, etc. to your database server, but there are hardware limits. If you have a large user base, a single server is not enough.

- Greater risk of single point of failures.
- The overall cost of vertical scaling is high. Powerful servers are much more expensive.

### Horizontal scaling

Horizontal scaling, also known as sharding, is the practice of adding more servers

![[Untitled 17 4.png|Untitled 17 4.png]]

Sharding separates large databases into smaller, more easily managed parts called shards. Each shard shares the same schema, though the actual data on each shard is unique to the shard.

_**Example:**_

![[Untitled 18 4.png|Untitled 18 4.png]]

User data is allocated to a database server based on user IDs. Anytime you access data, a hash function is used to find the corresponding shard. In our example, user_id % 4 is used as the hash function.

![[Untitled 19 4.png|Untitled 19 4.png]]

The most important factor to consider when implementing a sharding strategy is the choice of the sharding key. Sharding key (known as a partition key) consists of one or more columns that determine how data is distributed.

A sharding key allows you to retrieve and modify data efficiently by routing database queries to the correct database. When choosing a sharding key, one of the most important criteria is to choose a key that can evenly distributed data.

### Sharding Challenges

_**Resharding data:**_

Resharding data is needed when

- a single shard could no longer hold more data due to rapid growth.
- Certain shards might experience shard exhaustion faster than others due to uneven data distribution.

When shard exhaustion happens, it requires updating the sharding function and moving data around.

_**Celebrity problem:**_ This is also called a hotspot key problem. Excessive access to a specific  
shard could cause server overload. For social applications, that shard will be overwhelmed  
with read operations.  

To solve this problem, we may need to allocate a shard for each celebrity. Each shard might even require further partition.

_**Join and de-normalization:**_ Once a database has been sharded across multiple servers, it is hard to perform join operations across database shards. A common workaround is to denormalize the database so that queries can be performed in a single table.

  

### Final Design Of 3-tier Web Architecture

![[Untitled 20 3.png|Untitled 20 3.png]]