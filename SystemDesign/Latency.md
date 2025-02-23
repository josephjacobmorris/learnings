# Latency
Latency in software design refers to the time it takes for a computer or network device to respond to a user input, request, or event. In other words, it's the delay between when a user interacts with a system (e.g., clicks a button, submits a form, or opens an application) and when the system responds with its output.

## Types of latency:

**System latency:** This refers to the time it takes for a computer or device to execute instructions, access data, or perform tasks.

**Network latency:** This is the delay between when a packet of data is sent over a network (e.g., internet) and when it arrives at its destination.

**User latency:** This refers to the time it takes for users to see the results of their interactions with a system.

## Impact of High Latency
* Can lead to bad user Experience and user frustration 
* Can lead to bad SEO rankings.

## Ways to reduce Latency

* Caching
* Content Delivery Network
* Load Balancers
* Async Task Processing
* Database Indexing
* Pre-warming caches

## Latency Reference for calculation approximation for interviews
| Communication Type                   | Latency |
|--------------------------------------|---------|
| L1 Cache                             | 1ns     |
| L2 Cache                             | 10ns    |
| RAM Reference                        | 100ns   |
| SSD Random Read                      | 100us   |
| Packet round trip same region        | 1ms     |
| HDD Disk Seek                        | 10ms    |
| Packet round trip between continents | 100ms   |




## References
* https://blog.bytebytego.com/p/top-strategies-to-reduce-latency?utm_source=share&utm_medium=android&r=40ln3j&triedRedirect=true
* https://substack.com/@sidm0/note/c-81156846