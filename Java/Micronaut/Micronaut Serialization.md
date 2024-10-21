# Micronaut Serialization

## Introduction

Advantages of using micronaut serialization
* reduced image size (Micronaut Serialization = 380kb JAR , Jackson Databind > 2mb)
* Security as arbitary serialization/deserialization is not possible. 
* compile time validation instead of runtime execution in Jackson
* micronaut serialization decouples from the underlying lib. At run time we can choose Jackson , JSON-B annotations or BSON annotations.

## References
* https://micronaut-projects.github.io/micronaut-serialization/latest/guide/