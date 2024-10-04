# Kafka

## Introduction

### Advantages 
1. **Asynchronous processing in Kafka** refers to the ability of producers and consumers to operate independently without waiting for one another, allowing tasks to be processed concurrently.
2. **Fault Tolerance**: If a consumer fails, Kafka retains the messages, allowing processing to resume once the consumer is back online.
3. **Scalability**: Asynchronous processing allows the system to handle larger loads by adding more consumers or producers without changing the overall design. Producers and consumers can be scaled without blocking each other.
4. **Fault Tolerance**: Kafka replicates data across multiple brokers, ensuring reliability even in case of node failures.

### Kafka Childrens Book
https://www.gentlydownthe.stream/

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

## Challenges with Kafka

### Consumer Group Rebalancing

**Consumer Group Rebalancing** is the process of redistributing partitions among consumers in a consumer group. This ensures that all partitions of a topic are evenly distributed across the available consumers in the group, allowing for optimal message consumption. Rebalancing occurs under several circumstances, including:

- **New Consumers Join**: When a new consumer joins an existing consumer group, the partitions need to be redistributed among all active consumers.

- **Consumers Leave**: If a consumer crashes, disconnects, or is manually removed from the group, the remaining consumers must take over the partitions that were assigned to the departed consumer.

- **Partition Count Changes**: If the number of partitions for a topic increases or decreases, the distribution of these partitions across the consumers must be adjusted.

- **Configuration Changes**: Changes to consumer configurations that require a rebalancing, such as changing the maximum number of consumers or altering session timeouts.

#### How Rebalancing Works

1. **Coordinating with the Kafka Broker**: Each consumer group has a designated **group coordinator**, which is a Kafka broker responsible for managing the consumer group. The group coordinator tracks the membership of the group and the assignments of partitions to consumers.

2. **Session Timeouts**: Consumers periodically send heartbeats to the broker to indicate they are alive. If the broker does not receive a heartbeat from a consumer within the configured session timeout, it considers the consumer as failed and triggers a rebalance.

3. **Rebalance Process**:
    - **Member Join/Leave**: When a consumer joins or leaves the group, the coordinator triggers a rebalance.
    - **Partition Assignment**: The coordinator assigns partitions to the active consumers in the group. This assignment can be done using different strategies, such as:
        - **Range Assignment**: Partitions are assigned in ranges based on the consumer IDs.
        - **Round Robin Assignment**: Partitions are assigned in a round-robin manner across all consumers.
        - **Sticky Assignment**: Tries to minimize the movement of partition ownership to maintain stability.
    - **Commit Offsets**: Before a rebalance, consumers commit their offsets to ensure that they can resume processing messages from the correct position after the rebalance.

4. **Post-Rebalance**: Once the partitions are reassigned, consumers start processing messages from their newly assigned partitions.

#### Impact of Rebalancing

- **Temporary Disruption**: During rebalancing, consumers cannot process messages. This can lead to a temporary disruption in message consumption.
- **Increased Latency**: The time taken to rebalance can introduce latency in message processing.
- **Complexity**: Handling rebalances effectively requires careful management of offsets and state, especially in applications that maintain local state based on message processing.

#### Best Practices to Minimize Rebalance Impact

1. **Configure Session Timeouts**: Set appropriate session timeout values to avoid unnecessary rebalances caused by transient network issues.

2. **Use `enable.auto.commit` Wisely**: Consider managing offsets manually to control when offsets are committed, minimizing the chance of reprocessing messages after a rebalance.

3. **Optimize Consumer Performance**: Ensure that consumers can process messages quickly to reduce the duration of rebalancing.

4. **Monitor Consumer Group Health**: Use Kafka monitoring tools to keep track of consumer group status, message lag, and rebalance events.

### Poison Pill message
In Apache Kafka, a **poison pill message** refers to a message that causes a consumer to fail or behave unexpectedly when it is processed. This type of message can disrupt the normal processing flow, potentially leading to message consumption halting or creating issues within the application logic. Understanding poison pill messages is important for designing robust Kafka consumers and handling error scenarios effectively.

### Characteristics of Poison Pill Messages

1. **Error-Prone Data**: The message content may be malformed, corrupted, or not conforming to the expected format, which results in an error when processed.

2. **Special Values**: Sometimes, certain messages are deliberately designated as "poison pills" to signal special conditions. For example, a specific value (like `null`, `-1`, or a specific string) may be used to indicate that a consumer should stop processing or perform a specific action.

3. **Infinite Loop**: If a consumer does not handle errors correctly, a poison pill message can lead to infinite retries, where the consumer keeps attempting to process the same message without success.

### Impacts of Poison Pill Messages

- **Consumer Failures**: If a consumer encounters a poison pill message and doesn't handle it gracefully, it may crash or stop processing messages altogether.

- **Message Lag**: A poison pill can cause delays in message processing, leading to increased message lag as subsequent messages may not be processed until the issue is resolved.

- **System Resource Exhaustion**: Continuous processing failures due to poison pill messages can consume system resources (like CPU and memory) as the consumer repeatedly attempts to process the problematic message.

### Strategies to Handle Poison Pill Messages

1. **Error Handling Logic**:
    - Implement robust error handling within the consumer logic. This could include try-catch blocks to catch exceptions that occur during message processing.
    - Log errors for diagnostic purposes, so developers can identify and address the underlying issue with the poison pill.

2. **Dead Letter Queue (DLQ)**:
    - Use a **Dead Letter Queue** to store poison pill messages separately. When a consumer encounters an error, it can send the problematic message to a DLQ instead of continuously retrying it.
    - This allows the consumer to continue processing other messages while providing a way to investigate and resolve issues with the problematic messages later.

3. **Retries with Backoff**:
    - Implement a retry mechanism with exponential backoff to limit the number of retries and give time for transient issues to resolve themselves.
    - After a certain number of retries, the message can be sent to the DLQ.

4. **Message Validation**:
    - Validate messages before processing to ensure they conform to the expected format. This could involve schema validation using tools like **Schema Registry** with Avro or Protobuf.
    - If a message fails validation, it can be logged or sent to a DLQ instead of being processed.

5. **Use of Sentinel Values**:
    - If using special values as poison pills, clearly document these sentinel values and ensure that consumers recognize them and handle them appropriately.
    - For example, if a consumer encounters a message with a value of `null`, it can treat this as a signal to stop processing or perform a clean-up operation.

### Conclusion

Poison pill messages can pose significant challenges in Kafka-based systems, potentially leading to consumer failures and processing disruptions. By implementing effective error handling strategies, using dead letter queues, and performing message validation, developers can mitigate the impact of poison pill messages and maintain the reliability and resilience of their Kafka applications. Understanding the nature of poison pills is crucial for designing robust message processing workflows and ensuring system stability.

#### Conclusion

Consumer group rebalancing in Kafka is a vital process for ensuring that message consumption is distributed efficiently among consumers. While it provides the flexibility to adapt to changing workloads and consumer membership, it also introduces complexities and potential disruptions. Proper configuration and management practices can help mitigate the impact of rebalancing and maintain a stable, high-performing Kafka consumer group.


### How Partitions Are Tied to Consumer Scaling

1. **Parallelism**:
    - Each partition in a Kafka topic is an independent sequence of records that can be consumed by a consumer. This means that the number of partitions directly affects the level of parallelism in message consumption.
    - When a topic has multiple partitions, multiple consumers within the same consumer group can read from these partitions simultaneously. Each consumer in the group can be assigned to one or more partitions, allowing for concurrent processing of messages.

2. **Consumer Group Dynamics**:
    - In a consumer group, each partition can be assigned to only one consumer at a time. Therefore, the maximum number of consumers that can actively consume from a topic is limited by the number of partitions.
    - If there are more consumers than partitions, some consumers will remain idle, as they cannot be assigned to a partition.

3. **Scaling Out Consumers**:
    - To increase throughput, organizations can scale out by adding more consumers to a consumer group. However, to fully leverage this scaling, the number of partitions must also increase. If there are not enough partitions, simply adding more consumers will not yield performance benefits.
    - For instance, if a topic has only five partitions, adding ten consumers to the consumer group will result in only five consumers actively processing messages, with the other five being idle.

4. **Partition Assignment Strategy**:
    - Kafka provides several strategies for partition assignment, such as round-robin and sticky assignment. These strategies ensure that consumers are efficiently assigned to partitions, maximizing the consumption throughput and balancing the load across consumers.

### Cost Implications of Partitions and Consumer Scaling

1. **Infrastructure Costs**:
    - **Brokers and Partitions**: Each partition requires resources (CPU, memory, disk space) on the Kafka brokers. More partitions typically mean higher infrastructure costs, as organizations may need to provision more brokers to manage the increased load and maintain performance.
    - **Replication**: Kafka replicates partitions across multiple brokers for fault tolerance. The number of replicas increases the storage requirements and costs, as each partition consumes additional disk space proportional to the number of replicas.

2. **Operational Complexity**:
    - **Management Overhead**: More partitions can lead to increased complexity in managing the Kafka cluster. Administrators must monitor partition distribution, manage partition reassignment during rebalances, and ensure that consumers are effectively processing messages.
    - **Scaling Challenges**: If the number of partitions is not properly managed, organizations may face challenges in scaling their consumers effectively. Inefficient partition distribution can lead to load imbalances, causing some consumers to be overwhelmed while others are underutilized.

3. **Performance Tuning Costs**:
    - Organizations may need to invest time and resources into tuning their Kafka configurations to optimize performance based on the number of partitions and consumers. This can include configuring consumer group settings, adjusting partition counts, and implementing effective message retention policies.

4. **Increased Latency and Message Lag**:
    - As the number of partitions increases, the complexity of managing message consumption also grows. If consumers cannot keep up with the rate of incoming messages, it can lead to increased latency and message lag, potentially impacting the performance of downstream systems and applications.

5. **Storage Costs**:
    - With more partitions, the total amount of data retained in Kafka can increase, especially if the retention policy is set to keep messages for a longer period. This can lead to higher storage costs as more disk space is consumed to accommodate the data across multiple partitions.

### Conclusion

In summary, partitions in Kafka are integral to enabling consumer scaling and maximizing throughput. However, the number of partitions needs to be carefully balanced with the number of consumers to optimize performance and resource utilization. While increasing partitions can lead to higher throughput and better resource distribution, it also has significant cost implications related to infrastructure, operational complexity, performance tuning, and storage.

Organizations should plan their Kafka architectures with an understanding of these trade-offs, ensuring that they provision the appropriate number of partitions and consumers to meet their performance and cost objectives effectively.

## References

* Udemy 
* Chatgpt
* Substack - The polymathic engineer
*  Reactive Kafka example - https://github.com/Kevded/example-reactive-spring-kafka-consumer-and-producer/blob/master/README.md