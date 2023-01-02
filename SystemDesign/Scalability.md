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