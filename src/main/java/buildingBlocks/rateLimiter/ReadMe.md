# What is a Rate Limiter?
A rate limiter is a defensive mechanism used in a distributed system to prevent the frequency of an operation from exceeding a defined limit and causing server errors.

#### What Is Rate Limiting?
- Rate limiting protects your APIs from unintended or malicious overuse by limiting the number of requests that can reach your API in a given period of time. 
- Without rate limiting, any user can bombard your server with requests leading to spikes that starve other users i.e. risking resource starvation from load-based denial of service attacks, security failures and increased operational costs.
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/rate-limiter.png" alt="Image" style="display:block; width:70%; height:auto; margin:auto;">
</div>

<br><br>
## Why Use a Rate Limiter?
Below are three reasons to use a rate limiter: 
### 1. Preventing Resource Starvation 
The most common reason for rate limiting is to avoid resource starvation and improve the availability of API-based services. Rate limiting can also help you prevent load-based denial of service (DoS) attacks. Other users aren’t starved, even when one user bombards the API with loads of requests.

### 2. Security 
Rate limiting prevents users from brute forcing security intensive functionalities like login, promo codes, etc. The number of requests for each of these features is limited on a user level ensuring that brute force algorithms won’t work in these scenarios.

### 3. Preventing Operational Costs 
In the case of auto-scaling resources on a pay-per-use model, rate limiting puts a virtual cap on the scalping of resources to help control operational costs. Without rate limiting, resources might scale out of proportion leading to exponential bills.

<br><br>
## What Are the Different Types of Rate Limiting? 
- <b><i>User Rate Limiting:</i></b> A limit is applied on the number of requests allowed for a user in a given period of time. User-based rate limiting is one of the most common and intuitive forms of rate limiting.
- <b><i>Concurrency Rate Limiting:</i></b> Here the limit is employed on the number of parallel sessions that can be allowed for a user in a given timeframe. A limit on the number of parallel connections also helps mitigate distributed denial of service (DDoS) attacks.
- <b><i>Location and ID Rate Limiting:</i></b> This is useful when you’re running location-based or demography-centric campaigns. Requests from outside the target demography can be rate limited so as to increase availability in the target regions.
- <b><i>Server Rate Limiting:</i></b> Server-based rate limiting is a niche strategy. This is generally employed when specific servers need to handle most of the requests, such as servers that are strongly linked to executing specific functions.

<br><br>
## Common Rate Limiting Algorithms
### 1. Leaky Bucket
Leaky bucket is a simple, intuitive algorithm. It creates a queue with a finite capacity. All requests in a given time frame beyond the capacity of the queue are spilled off.
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/leaky-bucket-rate-limiter.jpg" alt="Image" style="display:block; width:40%; height:auto; margin:auto;">
</div>

- <b><i>Bucket Initialization:</i></b> Imagine a bucket with a fixed capacity, representing the maximum amount of data that can be transmitted or stored in a given time period.
- <b><i>Rate limit enforcement:</i></b> 
  - <u><i>Accept Request:</i></u> As data or requests arrive, they are added to the bucket.
  - <u><i>Request Delay or Denial:</i></u> If the bucket is already full, the incoming data is discarded or subjected to a specific handling policy (e.g., dropped or queued).
- <u><i>Bucket leak:</i></u> The bucket continuously “leaks” or drains at a fixed rate. This means that the data in the bucket is continuously being processed or transmitted, ensuring adherence to the desired rate limit. It is usually implemented with a first-in-first-out (FIFO) queue.
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/leaky-bucket-rate-limiting.jpg" alt="Image" style="display:block; width:80%; height:auto; margin:auto;">
</div>

#### Pros:
- It smoothes out bursts of requests and processes them at a constant rate. 
- It’s also easy to implement on a load balancer and is memory efficient for each user. 
- A constant, near-uniform flow is applied to the server irrespective of the number of requests.
#### Cons:
- The downside of this algorithm is that a burst of requests can fill up the bucket leading to the starvation of new requests.
- It also provides no guarantee that requests get completed in a given amount of time.

### 2. Token Bucket
The Token Bucket algorithm is a popular method for rate limiting and regulating the flow of requests or events in a system. It allows a system to handle bursts of activity while enforcing a specific rate limit over time. The algorithm operates based on the concept of a token bucket that replenishes tokens at a fixed rate.
- <b><i>Token Bucket Initialization:</i></b> A token bucket is a container that has a pre-defined capacity. The capacity represents the maximum number of tokens the bucket can hold.
<div align="center">
<<<<<<< HEAD
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/token-bucket-capacity.jpg" alt="Image" style="display:block; width:80%; height:auto; margin:auto;">
</div>

- <b><i>Token consumption:</i></b> Each request or event that arrives at the system needs to “consume” tokens from the bucket. The number of tokens consumed depends on the size or cost associated with the request.
- <b><i>Bucket refilling:</i></b> The token bucket refills at a fixed rate, adding tokens to the bucket over time. The refill rate determines how quickly the bucket replenishes its tokens.
- <b><i>Rate limit enforcement:</i></b>
  - <u><i>Accept Request:</i></u> If a request arrives and there are sufficient tokens available in the bucket, it is allowed to proceed, and the tokens are consumed.
  - <u><i>Request Delay or Denial:</i></u> If there are not enough tokens available, the request is either delayed or rejected, depending on the specific implementation.
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/token-bucket-rate-limiter.jpg" alt="Image" style="display:block; width:80%; height:auto; margin:auto;">
</div>

#### Pros:
- <u><i>Memory Efficient:</u></i> This algorithm is memory efficient because we are saving less data per user for our application.
- <u><i>Flexible rate limiting:</u></i> It provides flexibility in setting different rate limits for different types of requests or events by adjusting the token consumption rate or token size.
#### Cons:
- <u><i>Race Conditions:</u></i> The problem is that it can cause race conditions in a distributed environment. This happens when there are two requests from two different application servers trying to fetch the token at the same time.
- <u><i>Tuning:</u></i> Challenging to tune bucket size and token refill rate properly.

### 3. Fixed Window Counter
Fixed window is one of the most basic rate limiting mechanisms. We keep a counter for a given duration of time and continue incrementing it for every request we get. Once the limit is reached, we drop all further requests until the time duration is reset.
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/fixed-window-rate-limiter.jpg" alt="Image" style="display:block; margin:auto;">
</div>

- <b><i>Counter Initialization:</i></b> Start with a counter set to zero at the beginning of a fixed time window.
- <b><i>Event Counting:</i></b> Each time an event occurs, the counter is incremented by one.
- <b><i>Rate limit enforcement:</i></b>
  - <u><i>Accept Request:</i></u> When a new event arrives, check the value of the counter. If the counter is below the predefined limit, the event is allowed to proceed, and the counter is incremented.
  - <u><i>Request Delay or Denial:</i></u> If the counter is equal to or exceeds the limit, the event is blocked or limited in some way (e.g., dropped, delayed, or logged).
- <u><i>Time Window Reset:</i></u> At the end of the fixed time window, the counter is reset back to zero, allowing a fresh count for the next time window.

#### Pros:
- The advantage is that it ensures that the most recent requests are served without being starved by old requests.
#### Cons:
- However, a single burst of traffic right at the edge of the limit might hoard all the available slots for both the current and next time slot. Consumers might bombard the server at the edge in an attempt to maximize the number of requests served.

### 4. Sliding Log
A sliding log algorithm involves maintaining a time stamped log of requests at the user level. The system keeps these requests time sorted in a set or a table. It discards all requests with timestamps beyond a specified threshold. Every minute we look out for older requests and filter them out. Then we calculate the sum of logs to determine the request rate. If the request would exceed the threshold rate, then it is held. Otherwise, the request is served.

#### Pros:
- The advantage is that it does not suffer from the boundary conditions of fixed windows.
- Enforcement of the rate limit will remain precise. Since the system tracks the sliding log for each consumer, you don’t have the stampede effect that challenges fixed windows.
#### Cons:
- However, it can be costly to store an unlimited number of logs for every request.
- It’s also expensive to compute because each request requires calculating a summation over the consumer’s prior requests, potentially across a cluster of servers.
- As a result, it does not scale well enough to handle large bursts of traffic or denial of service attacks.

### 5. Sliding Window 
This is similar to the sliding log algorithm, but it’s more memory efficient. It combines the fixed window algorithm’s low processing cost and the sliding log’s improved boundary conditions.

With this algorithm, we keep a list/table of time sorted entries, each being a hybrid and containing the timestamp and the number of requests at that point. We keep a sliding window of our time duration and only service requests in our window for the given rate. If the sum of counters is more than the given rate of the limiter, then we take only the first sum of entries equal to the rate limit.

#### Pros:
- The sliding window approach is the best because it gives you the flexibility to scale rate limiting while still maintaining good performance.
- The rate windows are an intuitive way to present rate limit data to API consumers.
- It also avoids the starvation problem of the leaky bucket and the bursting problems of fixed window implementations.

<br><br>
## How to Apply a Rate Limiter in Distributed Systems
While those algorithms work well for single server applications, it’s a lot more complicated when there’s a distributed system with multiple nodes or app servers.  When there are multiple rate limited services distributed across different server regions, two problems arise:  
### 1. Inconsistency
When you have complex systems with multiple app servers distributed across different regions each with their own rate limiters, you need to define a global rate limiter.

A consumer could surpass the global rate limiter individually if it receives a lot of requests in a small time frame. The greater the number of nodes, the more likely the user will exceed the global limit.

There are two ways to solve for this problem:
- <b><i>a) Sticky session:</i></b> When you introduce a sticky session in your load balancers, each consumer gets sent to exactly one node. The downside to this strategy includes a lack of fault tolerance and scaling problems when nodes get overloaded.
- <b><i>b) Centralized data store:</i></b> You can use a centralized data store like Redis or Cassandra to handle counts for each window and consumer. While this adds latency, the flexibility it provides makes it an elegant solution.

### 2. Race conditions
Race conditions happen in a get-then-set approach with high concurrency. Each request gets the value of the counter then tries to increment it. But by the time that write operation is completed, several other requests have read the value of the counter, which is not correct. As a result, a larger number of requests are sent than what was intended. This can be mitigated using locks on the read-write operation, thus making it atomic. But this comes at a performance cost as it creates a bottleneck causing more latency.

<br><br>
## Throttling
- Throttling is a broader concept that involves controlling the rate of requests or processing to manage system load and ensure resources are used efficiently. It’s about slowing down the rate of requests or operations when the system is under heavy load or when the rate limit is close to being exceeded.
- Throttling can be defined at the application level and/or the API level. When a throttle limit is crossed, the server returns HTTP status “429 — Too many requests.”

#### There are three types of throttling:
- <b><i>Hard throttling:</b></i> The number of API requests cannot exceed the throttle limit.
- <b><i>Soft throttling:</b></i> In this type, we can set the API request limit to exceed a certain percentage. For example, if we have a rate limit of 100 messages a minute and a 10 percent exceed limit, our rate limiter will allow up to 110 messages per minute.
- <b><i>Elastic or dynamic throttling:</b></i> Under elastic throttling, the number of requests can go beyond the threshold if the system has some resources available. For example, if a user is allowed only 100 messages a minute, we can let the user send more than 100 messages a minute when there are free resources available in the system.

<br><br>
#### #Resource: [rate-limiter by builtin.com](https://builtin.com/software-engineering-perspectives/rate-limiter)
#### #Resource: [rate-limiter](https://medium.com/@abhirup.acharya009/all-you-need-to-know-about-rate-limiting-an-eagles-view-part-1-fc3eb441c00c)