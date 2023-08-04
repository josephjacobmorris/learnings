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
## References

* Udemy
* Chatgpt