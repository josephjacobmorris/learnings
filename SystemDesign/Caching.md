# Caching
**Caching: What, Why, How, and Points to Consider**

**What is Caching:**

Caching is the process of storing frequently used data or results so that they can be retrieved quickly without requiring additional processing or database queries. This technique is commonly used in web development, software applications, and databases to improve performance, reduce load times, and enhance user experiences.

**Why Use Caching?**

1. **Improved Performance**: Caching reduces the time spent on retrieving data from a database, server, or other external sources.
2. **Reduced Load Times**: By storing frequently accessed data in memory, caching decreases the number of requests made to external systems.
3. **Enhanced User Experiences**: Fast response times improve user satisfaction and engagement with an application.
4. **Increased Scalability**: Caching helps applications handle increased load by distributing the load across multiple instances or processes.

**How Caching Works:**

1. **Request Processing**: An application receives a request for data or a service.
2. **Cache Checking**: The application checks if the requested data is already cached in memory ( cache layer).
3. **Cache Retrieval**: If the data is found in the cache, it's retrieved and returned to the application.
4. **Database Query**: If the data is not found in the cache or requires an external database query, a new request is sent to the database.

**Points to Consider When Using Caching:**

1. **Cache Size and Lifetime**
    * Choose a suitable cache size based on your application's specific needs.
    * Set cache expiration times to avoid stale data accumulation.
2. **Cache Types**
    * Implement different types of caching mechanisms, such as:
        + Weak keys (e.g., HTTP headers)
        + Strong keys (e.g., session IDs)
        + Cache layers (e.g., Redis, Memcached)
3. **Caching Strategies**
    * Choose a caching strategy that balances performance and data integrity, such as:
        + Least Recently Used (LRU) or Time-To-Live (TTL) algorithms
        + Optimistic concurrency control
4. **Cache Optimization**
    * Regularly clean up expired or invalid cache entries.
    * Monitor cache sizes and adjust settings as needed to maintain optimal performance.
5. **Security Considerations**
    * Ensure that your caching mechanisms are secure and protected against unauthorized access.
    * Use authentication, authorization, and encryption (e.g., HTTPS) to safeguard sensitive data.
6. **Scalability and Performance**
    * Design a scalable cache architecture to handle increased loads and traffic.
    * Optimize cache performance by reducing the number of database queries or external requests.

## Caching strategies

### Read-Through
A read-through cache strategy is a caching approach where the most recently accessed data or resources are cached first, and if the next access is for the same resource, the previous cache entry can be reused. This approach is often used in databases, file systems, and other systems that require frequent reads of the same data.

Here's an overview of how it works:

**Key characteristics:**

1. **Read-through:** The most recently accessed data or resources are cached first.
2. **No caching for writes:** Write operations (e.g., inserting, updating, deleting) are not cached until after they have been committed to the underlying storage.
3. **Cache replacement policy:** When a new entry is added to the cache, it replaces an existing entry in the cache.

**How it works:**

1. The system reads data from the underlying storage and adds it to the cache with a timestamp indicating when it was accessed last (e.g., "read at time 10").
2. If the next access is for the same resource, the previous cached entry can be reused by checking if its timestamp is older than the current timestamp.
3. If an older entry is found and not expired, it is replaced with a new one.

**Advantages:**

1. **Improved performance:** By caching frequently accessed data, the system reduces the number of disk I/O operations and improves overall system performance.
2. **Reduced latency:** Faster cache hits lead to lower latency for reads.

**Disadvantages:**

1. **Increased memory usage:** More entries are stored in the cache, which can consume more memory.
2. **Cache coherence issues:** If multiple threads or processes access data concurrently, it may become inconsistent if the underlying storage is not properly managed.

**Common implementation strategies:**

1. **LRU (Least Recently Used) caching:** A simple and widely used approach where the least recently accessed entry is removed first when the cache reaches its capacity.
2. **RLRU (Randomized Least Recently Used) caching:** An extension of LRU caching that randomly selects an entry to be removed from the cache based on a random factor, such as the number of cache misses.
3. **ECC (Efficient Cache Coherence):** A more complex approach that uses multiple cache lines and writes to ensure coherence between cache lines.

### Cache Aside

### Write Around

### Write through

### Write back

## References
* LLMs
* https://blog.algomaster.io/p/top-5-caching-strategies-explained?r=643nm&utm_campaign=post&utm_medium=web
* https://www.alachisoft.com/resources/articles/readthru-writethru-writebehind.html