# DIFFERENT CACHING STRATEGIES
## What is Caching?
Caching is about storing data for future reference. Caching is one of the easiest ways to increase system performance. Databases can be slow and speed is an important factor in the world of computers. Caching allows you to efficiently reuse previously retrieved or computed data.

## When we use Caching?
1. The primary reason for caching is to make access to data less expensive. This can mean either:
* Monetary costs, such as paying for bandwidth or volume of data sent, or
* Opportunity costs, like processing time that could be used for other purposes.
2. Fast data access is an absolute necessity and a rising expectation for today's users: serving up information to users quickly can be the difference between your app being deleted or not

## How does Caching work?
The data in a cache is generally stored in fast access hardware such as RAM (Random-access memory) and may also be used in correlation with a software component. The primary purpose of the cache is to increase data retrieval performance by reducing the need to access the underlying slower storage layer.

## Caching Approaches:
### 1. Cache-Aside :
This is the most commonly used caching approach. The cache sits on the side and the application directly talks to both the cache and the database. The fundamental data retrieval logic can be summarized as follows:
1. The application first checks the cache.
2. If the data is found in the cache, we have a cache hit. The data is read and then returned to the client. If the data is not found in the cache, we have a cache miss. The application has to do some extra work. It queries the database to read the data, returns it to the client, and then stores the data in the cache so the subsequent reads for the same data results in a cache hit.

![image](https://user-images.githubusercontent.com/111187510/184936485-6052e217-d36b-4725-ad1b-a259fc121950.png)


**Pros and cons :** 

This approach has a couple of advantages :
* The cache contains only data that the application requests, which helps keep the cache size cost-effective.
* Implementing this approach is straightforward and produces immediate performance gains, whether you use an application framework that encapsulates lazy caching or your custom application logic.

A disadvantage when using cache-aside as the only caching pattern is that because the data is loaded into the cache only after a cache miss, some overhead is added to the initial response time because additional roundtrips to the cache and database are needed.

### 2. Read-Through Cache :
The read-through cache sits in line with the database. When there is a cache miss, it loads missing data from the database, populates the cache, and returns it to the application. Both cache-aside and read-through strategies load the data lazily, which means only when it is first to read.

![image](https://user-images.githubusercontent.com/111187510/184941179-c028e1be-fc10-45d5-9cda-345d7e65b79f.png)

**Pros and cons :**

Read-through caches work best for read-heavy workloads when the same data is requested many times.

The disadvantage is that when the data is requested for the first time, it always results in a cache miss and incurs the extra penalty of loading data to the cache.

### 3. Write-Through Cache
In this write strategy, data is first written to the cache and then to the database. The cache sits in line with the database and the writes always go through the cache to the main database.

![image](https://user-images.githubusercontent.com/111187510/184941872-71898e0c-3e9a-4fd4-8bff-d8b16ce50995.png)

**Pros and cons :**

The advantage of this strategy is not much when it is on its own because they introduce extra write latency. After all, data is written to the cache first and then to the main database. But when paired with read-through caches, we get all the benefits of read-through and we also get a data consistency guarantee, freeing us from using cache invalidation techniques.

### 4. Write-Around :
In this, data is written directly to the database and only the data that is read makes it way into the cache.

**Pros and cons :**

Write-around can be combined with a read-through and provides good performance in situations where data is written once and read less frequently or never. This pattern can be combined with cache-aside as well.

### 5. Write-Back / Write-Behind : 
In this, the application writes data to the cache which acknowledges immediately and after some delay, it writes the data back to the database.

![image](https://user-images.githubusercontent.com/111187510/184944638-bdff6538-adfa-462f-9f7a-5416a53ab5c9.png)

**Pros and cons :**

Write-back caches improve the write performance and are good for write-heavy workloads. When combined with read-through, it works well for mixed workloads, where the most recently updated and accessed data is always available in the cache. It is resilient to database failures and can tolerate some database downtime. If batching or coalescing is supported, it can reduce overall writes to the database, which decreases the load and reduces costs.

## References
1. [Caching patterns - Database Caching Strategies Using Redis](https://docs.aws.amazon.com/whitepapers/latest/database-caching-strategies-using-redis/caching-patterns.html#:~:text=Two%20common%20approaches%20are%20cache,the%20primary%20database%20is%20updated.)
2. [Caching Strategies and How to Choose the Right](https://codeahoy.com/2017/08/11/caching-strategies-and-how-to-choose-the-right-one/)
3. [Caching Techniques: One should know](https://bootcamp.uxdesign.cc/caching-techniques-one-should-know-603e09d2b298)




