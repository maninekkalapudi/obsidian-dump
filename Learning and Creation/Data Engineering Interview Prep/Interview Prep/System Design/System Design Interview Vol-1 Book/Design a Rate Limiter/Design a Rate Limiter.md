In a network system, a rate limiter is used to control the rate of traffic(requests) sent by a client or a service over a specified period. If the API request count exceeds the threshold defined by the rate limiter, all the excess calls are blocked.

_**Examples:**_

- A user can write no more than 2 posts per second.
- You can create a maximum of 10 accounts per day from the same IP address.
- You can claim rewards no more than 5 times per week from the same device.

### Benefits of using an API rate limiter:

- Prevent resource starvation caused by Denial of Service (DoS) attack. For example, Twitter limits the number of tweets to 300 per 3 hours. Google docs APIs have 300 per user per 60 seconds for read requests
- Reduce cost. Limiting excess requests means fewer servers and allocating more resources to high priority APIs.
- Prevent servers from being overloaded

### Step 1 - Understand the problem and establish design scope

**Candidate**: What kind of rate limiter are we going to design? Is it a client-side rate limiter or  
server-side API rate limiter?  
  
**Interviewer**: Great question. We focus on the _server-side API rate limiter_.  
  
**Candidate**: Does the rate limiter throttle API requests based on IP, the user ID, or other  
properties?  
  
**Interviewer**: The rate limiter should be flexible enough to support _different sets of throttle  
rules.  
_  
  
**Candidate**: What is the scale of the system? Is it built for a startup or a big company with a  
large user base?  
  
**Interviewer**: The system _must be able to handle a large number of requests_.  
  
**Candidate**: Will the system work in a _distributed environment_?  
  
**Interviewer**: Yes.  
  
**Candidate**: Is the rate limiter a separate service or should it be implemented in application  
code?  
  
**Interviewer**: It is a design decision up to you.  
  
**Candidate**: Do we need to inform users who are throttled?  
Interviewer: Yes.  

**Requirement Summary**

- _Accurately limit excessive requests._
- _Low latency_. The rate limiter should not slow down HTTP response time.
- _Use as little memory as possible._
- _Distributed rate limiting_. The rate limiter can be shared across multiple servers or  
    processes.  
    
- _Exception handling_. Show clear exceptions to users when their requests are throttled.
- _High fault tolerance_. If there are any problems with the rate limiter (for example, a cache  
    server goes offline), it does not affect the entire system.  
    

### Step 2 - Propose high-level design and get buy-in

**Where to put the rate limiter?**

- _**Client-side implementation**_: client is an unreliable place to enforce rate limiting because client requests can easily be forged by malicious actors as there is no control over the client implementation
- _**Server-side implementation**_: The webservers will host the rate-limiting logic along with backend application code

![[Untitled 49.png|Untitled 49.png]]

- _**Middleware implementation**_: The rate-limiter logic will be hosted on a seperated server that routes the requests based on allowed frequency
    
    ![[Untitled 1 22.png|Untitled 1 22.png]]
    
    Assume our API allows 2 requests per second, and a client sends 3 requests to the server within a second. The first two requests are routed to API servers and the middleware throttles the third request and returns a _**HTTP status code 429**_ (**too many requests**)
    

![[Untitled 2 21.png|Untitled 2 21.png]]

  

Where should the rater limiter be implemented, on the server-side or in a gateway? There is no absolute answer

- Evaluate your current technology stack, such as programming language, cache service,  
    etc. to implement rate limiting on the server-side within app servers.  
    
- Identify the rate limiting algorithm that fits your business needs.
- If you have already used microservice architecture and included an API gateway in the  
    design to perform authentication, IP whitelisting, etc., you may add a rate limiter to the  
    API gateway.  
    
- Building your own rate limiting service takes time, commercial API gateway is a better  
    option.  
    

### Algorithms for rate limiting

- _**Token bucket**_
- _**Leaking bucket**_
- _**Fixed window counter**_
- _**Sliding window log**_
- _**Sliding window counter**_

### Token bucket algorithm

A token bucket is a container that has pre-defined capacity. Tokens are put in the bucket at preset rates periodically. Once the bucket is full, no more tokens are added.

The token bucket capacity is 4 as in the below figure. The refiller puts 2 tokens into the bucket every second. Once the bucket is full, extra tokens will overflow.

![[Untitled 3 18.png|Untitled 3 18.png]]

- Each request consumes one token. When a request arrives, we check if there are enough  
    tokens in the bucket.  
    - If there are enough tokens, we take one token out for each request, and the request  
        goes through.  
        
    - If there are not enough tokens, the request is dropped.

![[Untitled 4 14.png|Untitled 4 14.png]]

In the below example, the token bucket size is 4, and the refill rate is 4 per 1 minute.

![[/Untitled 5 13.png|Untitled 5 13.png]]

The token bucket algorithm takes two parameters:

- _**Bucket size**_: the maximum number of tokens allowed in the bucket
- _**Refill rate**_: number of tokens put into the bucket every second

  

How many buckets do we need? This varies, and it depends on the rate-limiting rules:

- It is usually necessary to have different buckets for different API endpoints. For instance,  
    if a user is allowed to make 1 post per second, add 150 friends per day, and like 5 posts per  
    second, 3 buckets are required for each user.  
    
- If we need to throttle requests based on IP addresses, each IP address requires a bucket.
- If the system allows a maximum of 10,000 requests per second, it makes sense to have a  
    global bucket shared by all requests.  
      
    

**Pros**:

- The algorithm is easy to implement.
- Memory efficient.
- Token bucket allows a burst of traffic for short periods. A request can go through as long  
    as there are tokens left.  
    

**Cons**:

- Two parameters in the algorithm are bucket size and token refill rate. However, it might be challenging to tune them properly.

Amazon and Stripe use this algorithm to throttle their API requests.

### Leaking bucket algorithm

This is similar to the token bucket except that requests are processed at a fixed rate. It is usually implemented with a FIFO queue.

- When a request arrives, the system checks if the queue is full. If it is not full, the request  
    is added to the queue.  
    
- Otherwise, the request is dropped.
- Requests are pulled from the queue and processed at regular intervals.

![[Untitled 6 13.png|Untitled 6 13.png]]

Leaking bucket algorithm takes the following two parameters:

- _**Bucket size**_: it is equal to the queue size. The queue holds the requests to be processed at a fixed rate.
- _**Outflow rate**_: it defines how many requests can be processed at a fixed rate, usually in seconds.  
    Shopify uses leaky buckets for rate-limiting.  
    

_**Pros**_:

- Memory efficient given the limited queue size.
- Requests are processed at a fixed rate therefore it is suitable for use cases that a stable outflow rate is needed.

_**Cons**_:

- A burst of traffic fills up the queue with old requests, and if they are not processed in time, recent requests will be rate limited.
- There are two parameters in the algorithm. It might not be easy to tune them properly.

### Fixed window counter algorithm

- The algorithm divides the timeline into fix-sized time windows and assign a counter for each window.
- Each request increments the counter by one.
- Once the counter reaches the pre-defined threshold, new requests are dropped until a new  
    time window starts.  
    

Example:

In Figure 4-8, the time unit is 1 second and the system allows a maximum of 3 requests per second. In each second window, if more than 3 requests are received, extra requests are dropped as shown in Figure 4-8.

![[/Untitled 7 12.png|Untitled 7 12.png]]

  

When there is a burst of traffic at the edges of time windows could cause more requests than allowed quota to go through.

![[/Untitled 8 9.png|Untitled 8 9.png]]

In the above figure, the system allows a maximum of 5 requests per minute, and the available quota resets at the human-friendly round minute. As seen, there are five requests between 2:00:00 and 2:01:00 and five more requests between 2:01:00 and 2:02:00. For the one-minute window between 2:00:30 and 2:01:30, 10 requests go through. That is twice as many as allowed requests

  

_**Pros**_**:**

- Memory efficient.
- Easy to understand.
- Resetting available quota at the end of a unit time window fits certain use cases.  
      
    

_**Cons**_:

- Spike in traffic at the edges of a window could cause more requests than the allowed quota to go through.

### Sliding window log algorithm

The sliding window log algorithm fixes the issue of fixed window counter algorithm where it allows more requests to go through at the edges of a window.

- The algorithm keeps track of request timestamps. Timestamp data is usually kept in cache, such as sorted sets of Redis.
- When a new request comes in, remove all the outdated timestamps. Outdated timestamps are defined as those older than the start of the current time window.
- Add timestamp of the new request to the log.
- If the log size is the same or lower than the allowed count, a request is accepted. Otherwise, it is rejected.

_**Example**_:

![[/Untitled 9 8.png|Untitled 9 8.png]]

- The log is empty when a new request arrives at 1:00:01. Thus, the request is allowed.
- A new request arrives at 1:00:30, the timestamp 1:00:30 is inserted into the log. After the insertion, the log size is 2, not larger than the allowed count. Thus, the request is allowed.
- A new request arrives at 1:00:50, and the timestamp is inserted into the log. After the insertion, the log size is 3, larger than the allowed size 2. Therefore, this request is rejected even though the timestamp remains in the log.
- A new request arrives at 1:01:40. Requests in the range [1:00:40,1:01:40) are within the latest time frame, but requests sent before 1:00:40 are outdated. Two outdated timestamps, 1:00:01 and 1:00:30, are removed from the log. After the remove operation, the log size becomes 2; therefore, the request is accepted.  
      
    

_**Pros**_:

- Rate limiting implemented by this algorithm is very accurate. In any rolling window, requests will not exceed the rate limit.

_**Cons**_:

- The algorithm consumes a lot of memory because even if a request is rejected, its timestamp might still be stored in memory.

### Sliding window counter algorithm

The sliding window counter algorithm is a hybrid approach that combines the fixed window counter and sliding window log.

![[/Untitled 10 7.png|Untitled 10 7.png]]

Assume the rate limiter allows a maximum of 7 requests per minute, and there are 5 requests  
in the previous minute and 3 in the current minute. For a new request that arrives at a 30%  
position in the current minute, the number of requests in the rolling window is calculated  
using the following formula:  

- Requests in current window + requests in the previous window * overlap percentage of the rolling window and previous window
- Using this formula, we get 3 + 5 * 0.7% = 6.5 request. Depending on the use case, the number can either be rounded up or down. In our example, it is rounded down to 6.

Since the rate limiter allows a maximum of 7 requests per minute, the current request can go through. However, the limit will be reached after receiving one more request.

  

_**Pros:**_

- It smooths out spikes in traffic because the rate is based on the average rate of the  
    previous window.  
    
- Memory efficient.  
      
    

_**Cons:**_

- It only works for not-so-strict look back window. It is an approximation of the actual rate  
    because it assumes requests in the previous window are evenly distributed. However, this  
    problem may not be as bad as it seems. According to experiments done by Cloudflare [10],  
    only 0.003% of requests are wrongly allowed or rate limited among 400 million requests.  
    

  

### High-level architecture

At the high-level, we need a counter to keep track of how many requests are sent from the same user, IP address, etc. If the counter is larger than the limit, the request is disallowed.

Where shall we store counters? Using the database is not a good idea due to slowness of disk access. In-memory cache is chosen because it is fast and supports time-based expiration strategy. For example, Redis

Redis is an inmemory store that offers two commands: INCR and EXPIRE

- INCR: It increases the stored counter by 1.
- EXPIRE: It sets a timeout for the counter. If the timeout expires, the counter is  
    automatically deleted.  
    

![[/Untitled 11 7.png|Untitled 11 7.png]]

The client sends a request to rate limiting middleware. Rate limiting middleware fetches the counter from the corresponding bucket in Redis and checks if the limit is reached or not.

- If the limit is reached, the request is rejected.
- If the limit is not reached, the request is sent to API servers. Meanwhile, the system  
    increments the counter and saves it back to Redis  
    

### Step 3 - Design deep dive

The high-level design does not answer the following questions:

- How are rate limiting rules created? Where are the rules stored?
- How to handle requests that are rate limited?

### Rate limiting rules

Lyft open-sourced their rate-limiting component. Following are the rules for rate limiting

- This allow a maximum of 5 marketing messages per day

```YAML
domain: messaging
descriptors:
 - key: message_type
	 Value: marketing
	 rate_limit:
	 unit: day
	 requests_per_unit: 5
```

- Clients are not allowed to login more than 5 times in 1 minute

```YAML
domain: auth
descriptors:
- key: auth_type
	Value: login
	rate_limit:
	unit: minute
	requests_per_unit: 5
```

Rules are generally written in configuration files and saved on disk.

### Exceeding the rate limit

In case a request is rate limited, APIs return a HTTP response code 429 (too many requests) to the client

Depending on the use cases, we may enqueue the rate-limited requests to be processed later. For example, if some orders are rate limited due to system overload, we may keep those orders to be processed later

  

**Rate limiter headers**

How does a client know whether it is being throttled? And how does a client know the number of allowed remaining requests before being throttled?

The rate limiter returns the following HTTP headers to clients:

_**X-Ratelimit-Remaining**_: The remaining number of allowed requests within the window.

_**X-Ratelimit-Limit**_: how many calls the client can make per time window.

_**X-Ratelimit-Retry-After**_: The number of seconds to wait until you can make a request again without being throttled.

  

**Detailed design**

![[/Untitled 12 7.png|Untitled 12 7.png]]

- Rules are stored on the disk. Workers frequently pull rules from the disk and store them in the cache.
- When a client sends a request to the server, the request is sent to the rate limiter middleware first.
- Rate limiter middleware loads rules from the cache. It fetches counters and last request  
    timestamp from Redis cache. Based on the response, the rate limiter decides:  
    - if the request is not rate limited, it is forwarded to API servers.
    - if the request is rate limited, the rate limiter returns 429 too many requests error to  
        the client. In the meantime, the request is either dropped or forwarded to the queue.  
        

  

### Rate limiter in a distributed environment

Two challenges:

- **Race condition**
- **Synchronization issue**

  

**Race condition**

Race conditions can happen in a highly concurrent environment as show below

![[/Untitled 13 5.png|Untitled 13 5.png]]

Assume the counter value in Redis is 3. If two requests concurrently read the counter value before either of them writes the value back, each will increment the counter by one and write it back without checking the other thread.

Both requests (threads) believe they have the correct counter value 4. However, the correct counter value should be 5.

Locks are the most obvious solution for solving race condition. However, locks will significantly slow down the system. **Lua script** and [**sorted sets data structure**](https://redis.io/docs/data-types/sorted-sets/) in Redis are commonly used to solve the problem:

  

**Synchronization issue**

To support millions of users, one rate limiter server might not be enough to handle the traffic. When multiple rate limiter servers are used, synchronization is required

For example, client 1 sends requests to rate limiter 1, and client 2 sends requests to rate limiter 2.

![[/Untitled 14 5.png|Untitled 14 5.png]]

As the web tier is stateless, clients can send requests to a different rate limiter as shown on the right side of the above figure. If no synchronization happens, rate limiter 1 does not contain any data about client 2. Thus, the rate limiter cannot work properly.

One possible solution is to use sticky sessions that allow a client to send traffic to the same rate limiter. This solution is not advisable because it is neither scalable nor flexible.

A better approach is to use centralized data stores like Redis.

![[/Untitled 15 5.png|Untitled 15 5.png]]