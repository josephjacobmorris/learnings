# Kafka

## Introduction

## APIs

### Producer API

### Consumer API

### Connect API

### Streams API

## CLI Commands
**1. Create a Topic:**
You need to create a topic before producing or consuming messages.

```sh
kafka-topics.sh --bootstrap-server localhost:9092 --topic my-topic --create --partitions 3 --replication-factor 1
```

- `--bootstrap-server`: Specifies the Kafka broker(s) to connect to.
- `--topic`: Name of the topic to create.
- `--create`: Indicates that you want to create a topic.
- `--partitions`: Number of partitions for the topic.
- `--replication-factor`: Number of replicas for each partition.

**2. Produce Messages:**
You can use the Kafka producer to send messages to a topic.

```sh
kafka-console-producer.sh --bootstrap-server localhost:9092 --topic my-topic
```

This will open an interactive shell where you can enter messages. Press `Ctrl + D` to exit.

**3. Consume Messages:**
You can use the Kafka consumer to read messages from a topic.

```sh
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my-topic --from-beginning
```

- `--from-beginning`: Start consuming messages from the beginning of the topic.

## Architecture

### Partitioner

| Aspect                          | Partitioner with Key                                     | Partitioner without Key                            |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|
| Message Processing              | Hash-based on Key                                        | Round-Robin Assignment                             |
| Key Presence                    | Key is required                                          | Key is not required                                |
| Consistent Assignment           | Yes, maintains consistency                               | No, each partition used sequentially               |
| Load Balancing                  | Yes, evenly distributed                                  | Yes, prevents hotspots                             |
| Ordering Preservation           | Yes, messages with same key go to same partition         | N/A                                                |
| Message Distribution Efficiency | High efficiency                                          | Efficient                                          |
| Use Cases                       | When data needs to be consistently assigned based on key | When even distribution across partitions is needed |

### Controller
The controller in Kafka plays a crucial role in managing the overall state and coordination of the Kafka cluster. It is responsible for several important tasks that ensure the cluster's health, availability, and proper functioning. Here's an explanation of the role of the controller:

1. **Leader Election**: One of the key responsibilities of the controller is to manage leader elections for each partition across the cluster. It monitors the health of broker nodes and detects when a broker becomes unavailable. In such cases, it initiates leader elections to select a new leader for the affected partitions, ensuring data availability and fault tolerance.

2. **Partition Replicas Management**: The controller oversees the assignment and reassignment of replicas (copies) of partitions to broker nodes. It ensures that replicas are evenly distributed across brokers for load balancing and fault tolerance. When new brokers are added or existing ones are removed, the controller updates replica assignments accordingly.

3. **Topic Management**: The controller handles the creation, deletion, and configuration changes of topics within the Kafka cluster. It maintains metadata about topics, including their partition count, replication factor, and configuration settings.

4. **Controller Failover**: In case the controller node itself fails, Kafka employs an automatic failover mechanism. Another broker takes over the controller role, ensuring that cluster management operations continue seamlessly.

5. **Cluster Health Monitoring**: The controller constantly monitors the health of broker nodes, partitions, and replicas. It keeps track of brokers' online/offline status and detects potential issues, triggering corrective actions when necessary.

6. **Metadata Management**: The controller maintains metadata about partitions, replicas, brokers, and topics. This metadata is essential for routing and directing producer and consumer requests to the appropriate brokers.

In essence, the controller is like the orchestrator of the Kafka cluster, overseeing leader elections, partition management, broker health, and other critical cluster-wide operations. Its role is pivotal in maintaining data availability, reliability, and the smooth operation of Kafka's distributed architecture.

## Concepts

### Consumer Offsets
Consumer offsets in Kafka represent the position up to which a consumer group has successfully read messages from a partition within a topic. These offsets serve as checkpoints, enabling consumers to resume reading from the last processed position in case of failures or restarts. Kafka maintains and tracks these offsets automatically, ensuring that each consumer group can effectively manage its progress and avoid reprocessing messages. This mechanism offers fault tolerance and scalability, allowing multiple consumers within a group to work concurrently without duplicating or missing messages. By managing offsets, Kafka provides reliable message consumption and processing, making it possible to maintain data integrity and achieve efficient, distributed data processing across various applications and services.

### Consumer Groups
Consumer groups in Kafka are a mechanism that enables parallel and distributed processing of messages by allowing multiple consumers to work together as a group. Each consumer in a group reads data from one or more partitions of a topic. The key objectives of consumer groups are:

1. **Parallelism**: Consumer groups enable multiple consumers to process messages concurrently from different partitions, effectively increasing data processing throughput.

2. **Load Distribution**: Kafka automatically balances the load among consumers in a group, ensuring that each consumer processes a fair share of messages.

3. **Scalability**: Consumer groups enable horizontal scalability by adding more consumers to a group, distributing the workload and enhancing data processing capabilities.

4. **Fault Tolerance**: If a consumer in a group fails, Kafka redistributes its partitions to the remaining consumers, ensuring uninterrupted message processing.

5. **Exactly-Once Semantics**: Kafka ensures that each message is delivered to only one consumer in a consumer group, enabling exactly-once message processing semantics.

6. **Stateful Processing**: Consumer groups maintain the offset (position) of each consumer, allowing them to resume processing from where they left off, even after restarts.

7. **Multi-Tenancy**: Different applications or components can independently consume messages from the same topic using separate consumer groups, ensuring isolation and modularity.

8. **Complex Event Processing**: Consumer groups enable complex event processing scenarios, where different consumers perform distinct processing tasks on the same stream of data.

Consumer groups are a fundamental concept in Kafka, promoting efficient and reliable distribution of data processing tasks across a distributed system while providing fault tolerance and scalability.

### **Commit Log in Kafka:**

A commit log in Kafka refers to the durable and sequentially ordered record of all published messages within a Kafka cluster. It serves as the backbone of data persistence and fault tolerance in Kafka's architecture. Each topic partition has its own commit log, and messages are appended to the log as they are produced. This allows for high-throughput and low-latency data storage and retrieval. Consumers use committed offsets in the commit log to keep track of their progress, ensuring reliable and accurate data consumption. The commit log structure enhances data durability, enables efficient replayability, and contributes to Kafka's robust and scalable event streaming capabilities. The commit log in Kafka is present within each topic partition on the Kafka brokers. Each partition maintains its own commit log, which is a sequentially ordered and immutable record of all messages produced to that partition. This commit log is stored on the local file system of the broker.


### **Retention Policy in Kafka:**

Retention policy in Kafka refers to the configuration that determines how long messages are retained within a topic's partition commit log. It specifies the duration or size of data that Kafka will retain before potentially discarding or deleting it. Kafka supports two primary retention policies: time-based retention and size-based retention. Time-based retention retains messages for a specified time period, while size-based retention retains a specific amount of data before older messages are discarded. Retention policies allow efficient management of storage resources, data lifecycle, and compliance requirements. By setting appropriate retention policies, users can ensure that Kafka maintains the desired balance between data availability, storage costs, and operational needs.
#### Configuring Retention Period:

The retention period in Kafka can be configured using the log.retention.ms property for time-based retention, and the log.retention.bytes property for size-based retention. These properties can be set in the Kafka broker configuration (server.properties) or for specific topics.

* log.retention.ms: Specifies the maximum time in milliseconds that a message will be retained in the commit log.
* log.retention.bytes: Specifies the maximum size in bytes that the commit log for a partition can grow before older messages are discarded.

#### Default Retention Values:

The default retention period values are as follows:

log.retention.ms: 7 days (1 week)
log.retention.bytes: -1 (infinite, i.e., size-based retention is not enabled by default)
These default values can be modified according to the data retention requirements and storage capacity of the Kafka cluster. By adjusting these configuration settings, administrators can control how long data is retained in Kafka topics, managing storage utilization and data availability effectively.
## References

* Udemy 
* Chatgpt