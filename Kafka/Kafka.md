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

## References

* Udemy
* Chatgpt