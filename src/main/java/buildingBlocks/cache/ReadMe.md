# Introduction to Caching and Caching Strategies

## What is Caching?
Caching is the process of storing <b><i>frequently accessed data in a temporary high speed storage</i></b> called a cache to improve the speed of data access.

<br><br>
## What is Cache?
A cache is a <b><i>high-speed storage</i></b> that stores a small proportion of critical data so that requests for that data can be served faster.

<br><br>
## How caching works?
- <b><i>Cache Hit:</i></b> If the requested data is found in cache, it is returned directly.
- <b><i>Cache Miss:</i></b> If the requested data is not found in cache, it is retrieved from the source database, stored in the cache, and then returned.

**Note:** We always try to optimize cache for high cache hits and low cache miss.
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/cache-working.png" alt="Image" style="display:block; margin:auto;">
</div> 

### Real Life Examples of Caching
- **Web Browsers (Client Side Caching):** Store frequently accessed HTML, CSS, Javascript and images.
- **Content Delivery Networks (CDNs):** Store static files like videos and images closer to the user.
- **DNS Caching:** Stores the IP address of a domain name for faster retrieval.
- **Web Server Caching:** Reverse proxies can serve static and dynamic content directly. Web servers can also cache requests, returning responses without having to contact application servers.
- **Database Caching:** Database usually includes some level of caching in a default configuration, optimized for generic use case. Tweaking these settings for specific usage patterns can further boost performance.
- **Application Caching:** In-memory cache such as Memcached and Redis are key-value stores between your application and your data storage. Since the data is held in RAM, it is much faster than typical databases where data is stored on disk. RAM is more limited than disk, so cache invalidation algorithms such as Least Recently Used(LRU) can help invalidate 'cold' entries and keep 'hot' data in RAM.

<br><br>
## Levels of Caching
There are multiple levels you can cache that fall into two general categories -> **database queries** and **objects**:
- Row-level
- Query-level
- Fully-formed serializable objects
- Fully-rendered HTML

### Caching at the Database query level
Whenever you query the database, hash the query as key and store the result to the cache. This approach suffers from expiration issues:
- Hard to delete a cached result with complex queries.
- If one piece of data changes such as a table cell, you need to delete all cached queries that might include the changed cell.

### Caching at object level
See your data as an object, similar to what you do with your application code. Have your application assemble the dataset from the database into a class instance or a data structure(s).
- Removing the object from cache if its underlying data has changed.
- Allows for asynchronous processing: workers assemble objects by consuming the latest cached object.


### Suggestions for what to cache:
- User sessions
- Fully rendered web pages
- Activity streams
- User graph data

<br><br>
## Types of Caching Strategies

When data in the database is constantly being updated, it is important to ensure that the cache is also updated to reflect the changes. Otherwise, the application will serve outdated or stale data. So, we use cache invalidation techniques or caching strategies to maintain cache consistency.

## 1. Reads
### i. Cache-aside Strategy
- The application code manages the cache and database independently, the cache does not interact with the storage directly.
- <b><i>cache miss</i></b>: data is retrieved from the database and stored in the cache.

#### Disadvantages of Cache-aside:
- Each cache miss results in 3 trips, which can cause a noticeable delay.
- Data can become stale in cache if it is updated in the database. This issue is mitigated by setting a **time-to-live(TTL)** which forces an update of the cache entry, or by using write-through cache.
- When a node fails, it is replaced by new empty node, increasing latency.

### ii. Read-through Strategy
- The cache sits between the application code and database.
- <b><i>cache miss</i></b>: data is retrieved from the database, stored in the cache, and returned to the application.

**Use Case:** Read heavy system

#### What is difference between Cache-aside and Read-through?
In cache-aside, application code is responsible for retrieving data from the database and storing it in cache. In read through, this is supported by the cache provider.

So read-through strategy simplifies the application code by abstracting away the complexity of cache management.

## 2. Writes
### i. Write-through Strategy
- Writes are first made to the cache and then to the database.
- **Process:**
   - Application adds/updates entry in cache
   - Cache synchronously writes entry in data store
   - Return
- **Characteristics:**
   - Ensures consistency between the cache and database.
   - Trade off: Increases write latency
 
**UseCase:** Read heavy applications, with infrequent writes.
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/write-through-cache-strategy.png" alt="Image" style="display:block; width:50%; height:auto; margin:auto;">
</div> 

#### Disadvantages of Write-through
- When a new node is created due to failure or scaling, the new node will not cache entries until the entry is updated in the database. Cache-aside in conjunction with write though can mitigate this issue.
- Most data written might never be read, which can be minimized by TTL.

### ii. Write-around Strategy
- Writes bypass the cache and go directly to the database.
- Reduces write operations on the cache.
- Data in cache may not be up-to-date.

**Use Case:** Write heavy applications, infrequent reads.

### iii. Write-back Strategy (Write-behind Strategy)
- Writes are temporarily stored in the cache and then asynchronously written to database.
- Improves write performance. Lower write latency and higher write throughput.
- Tradeoff:
  - Carries the risk of data loss if the cache layer fails.
  - It is more complex to implement write-behind than it is to implement cache-aside or write-through.

**Use Case:** Write heavy applications
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/write-back-cache-strategy.png" alt="Image" style="display:block; width:70%; height:auto; margin:auto;">
</div> 

<br><br>
## Cache Eviction Policies
When the cache is full, some data needs to be removed to make room for new data. Common policies include:
- <b><i>Least Recently Used (LRU):</i></b> Removes the least recently used data.
- <b><i>Least Frequently Used (LFU):</i></b> Removes the least frequently used data
- <b><i>Most Recently Used (MRU):</i></b> Removes the most recently used data.
- <b><i>Random Replacement (RR):</i></b> Randomly selects and removes data.
- <b><i>First In First Out (FIFO):</i></b> Removes the oldest data.

<br><br>
## Different types of Caching used in System Design
#### i. Browser Caching
Stores resources like HTML, CSS, Javascript files in the web browser for faster retrieval on subsequent visits.

#### ii. Web Server Caching
Stores resources on the server side to reduce the load on application server.
Implementations:
- <b><i>Reverse Proxy Cache:</i></b> Acts as an intermediary between the browser and the web server.
- <b><i>Key-Value Store/DB:</i></b> Caches application data accessed by the application code.(Memcached or Redis)

**NOTE:** Unlike reverse-proxies, which cache HTTP responses for specific requests, key-value databases can cache any user-specific data or frequently accessed data on need.

#### iii. Content Delivery Network (CDN)
Improves delivery speed of static content by storing it in proxy servers located close to use around the world.

#### iv. Distributed Caching
Uses multiple caching servers spread across a network to scale beyond the memory limits(cache size) of a single machine.

**Data Distribution:** Each caching server maintains a portion of the cached data, and requests for data are directed to the appropriate server based on a hashing algorithm or some distribution strategy.

**Benefits:**
- faster response times
- scalability
- increased availability
- better fault tolerance
- 
**Use Case:** Large scale applications

<br><br>
## Advantages and Limitations of Caching
- <b><i>Faster Access:</i></b> Reduces the need to retrieve data from slower data sources.
- <b><i>Increased Throughput:</i></b> Provides much higher request rates compared to the main database.
- <b><i>Scalability:</i></b> Improves scalability by offloading backend databases.
- <b><i>Scalability:</i></b> Reduces overall system cost by offloading backend databases.
- <b><i>Scalability:</i></b> Allows access to data even when the network connection is unreliable or offline.
  
<br><br>
## Disadvantages and Limitations of Caching
- <b><i>Data Inconsistency:</i></b> Need to maintain consistency between caches and the source of truth such as the database through cache invalidation.
- <b><i>Cache Misses:</i></b> Introduce latency.
- <b><i>Initial Cache Warm-up:</i></b> Can cause temporary performance degradation.
- <b><i>Limited Write Performance:</i></b> Most caching strategies do not improve write performance.


<br><br>
#### #Resource: [www.enjoyalgorithms.com/blog/caching-system-design-concept](https://www.enjoyalgorithms.com/blog/caching-system-design-concept/)