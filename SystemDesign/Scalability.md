# Scalability

## Introduction

Scalability is the measure of how many concurrent requests our application can handle. There are two kinds of scaling strategies namely vertical scaling and horizontal scaling. Both help in increasing the number of concurrent requests which can be handled by the application.

## Vertical Scaling

It is the pratice of increasing the resources of the system such as RAM, CPU and storage. It is also referred to as scaling up. It is easier since the logic does'nt change. However it can be only increased to the limits of the server. It is highly expensive in the long run.

## Horizontal Scaling

It is the pratice of increasing the instances of the application running and splitting the load between different servers. It is also referred to as scaling out.
It gives scalability as well as resiliency. It is the preferred way for micro-service architectures. It is best for stateless applications. If the application is stateful , we need to consider more things like the CAP theorm. It requires load balancers to distribute the traffic across multiple instances.

### Load Balancer

Load Balancer is the software which handles distributing the traffic (incoming requests) across the sever cluster. It usually balances the traffic using consistent hashing algorithms. Nginx is one the most popular load balancer in the industry. Load Bqlancers should be fault tolerant and improves the availability. Consistent Hashing Algorithms accounts for machine failures and is easy to reduce/increase the instances , making it the most preferred load balancing algorithm.

#### Key range partition
data is partitioned based on range of keys. 

**Cons**:
Uneven key distribution.

### Static Hash Partitioning
Data is partitioned using a hash function.

**Cons**:
* Not horizontally scalable
* data has to be rehashed when number of node changes


#### Consistent Hashing Algorithm

Normal Hashing Algorithms distribute data evenly but when the number of server changes (as in when server goes down or a new server is added) almost all data needs to be redistributed. In consistent Hashing Algorithm we consider a circular queue and the server are hashed and stored randomly. Then the keys (data) is also hashed and it is stored in
server whih comes next to the key in the clock-wise direction and hence in case the number of servers change only a fraction of data needs to be redistributed. Since the servers are randomly distributed across the circular queue data might not be evenly distributed across them. So to make the distribution more even we add in virtual nodes.

## References

* <https://www.youtube.com/watch?v=UF9Iqmg94tk&ab_channel=ByteByteGo>
* https://systemdesign.one/consistent-hashing-explained/