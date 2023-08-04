# Kafka

## Introduction

## APIs

### Producer API

### Consumer API

### Connect API

### Streams API

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