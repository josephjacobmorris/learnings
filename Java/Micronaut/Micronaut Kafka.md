# Micronaut Kafka
## Coding How to Syntax
**Add dependency**
```groovy
compile 'io.micronaut.configuration:micronaut-kafka'
```
**Configuration**
```yaml
kafka:
    bootstrap:
        servers:
#          value can be list of hosts
            - foo:9092
            - bar:9092
```
|     Annontation      |                                        Description                                        | Exemple                                                                                                |
|:--------------------:|:-----------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------|
|    `@KafkaClient`    |                               Used to create kafka producer                               | `@KafkaClient(batch=true)`                                                                             |
|   `@KafkaListener`   |                               Used to create kafka consumer                               | `@KafkaListener(groupId="myGroup", threads=10)`                                                        |
| `@Topic("my-topic")` |              Topic for Kafka.We can mention multiple topics and regexes also              | `@Topic("my-topic")`, `@Topic("fun-products", "awesome-products")`, `@Topic(patterns="products-\\w+")` |
|     `@KafkaKey`      | Key for Kafka. It can be omitted but it means kafka wont know how to partition the record | `@KafkaKey(value = "my-key")`                                                                          |
|      `@Header`       |                         Used to  define headers for kafka message                         |                                                                                                        |
|       `@Body`        |                       used to indicate the body message to be sent                        |                                                                                                        |

## Testing How to
**Embedded Kafka for test**
```groovy
testCompile 'org.apache.kafka:kafka-clients:2.1.1:test'
testCompile 'org.apache.kafka:kafka_2.12:2.1.1'
testCompile 'org.apache.kafka:kafka_2.12:2.1.1:test'
```
## References
* https://micronaut-projects.github.io/micronaut-kafka/1.1.x/guide/