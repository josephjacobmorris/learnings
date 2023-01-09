# Micro-services Architecture

## Introduction

Micro-services architecture is a sytle of architecture in which in single application is developed as a suite of services.

## Features

* Small, loosely coupled services which can be handled by a small team.
* Seperate codebase for different services
* Deployed independently
* Storing their own data
* Technology agnostic
* Services communicate via well defined APIs
* Different programming languages can be used.
* Services can be scaled independently
* Easier to implement features and bug fixes
* Fault isolation is easy
* Uses Database per Service Design Pattern

## Characteristics

* Componentization via service
* Products not Projects
* Decentralized Goverance - Netflix follows this characteristic where libraries are made open source.
* Infrastructure Automation
* Design for Failure.

## Challenges

* Complexity - Simple services but entire system is complex
* Network problems and latency
* Development and testing e2e when compared monolithic.
* Data Integrity - We should aim for eventual consistency and transaction are hard to implement

## Decomposition of micro-services

**Steps:**

* Domain Analysis
* Bounded Contexts
* Decompose Strategies 
* Identify Micro-service Boundaries

### Decomposition by Buisness Capablilities:

**Characteristics of m8s :**

* must be cohesive
* must be loosely coupled
* according to buisness capabilities

## Communication

* Sync or Async communication ?  
* Uses http (client - m8s) or gRPC (inter -m8s) or message brokers.

|Rest API | gRPC|
|:-------:|:----|
|Slower | Faster|
|uses Json|uses protobuf|
|payloads are human readable | payloads are not human readable|

### Synchronous communication

#### REST

REST (Representational Stateless Transfer)

Features

* Stateless
* Cacheable
* Code on Demand
* Uniform Interface
* Layered System

Richardson Maturity Model

* Level 0 : One URI
* Level 1 : Create seperate URIs
* Level 2 : Use HTTP methods
* Level 3 : Use hypermedia

#### API Versioning

use v1,v2,v3 etc for REST URI so that in future when API gets changed 

* It doesnt break some communication
* backward compatible and it will not break any communication
* Best Pratice. 

#### gRPC (gRPC Remote Procedure Call)

* Uses HTTP/2 protocol
* Open source
* Binary serialization

### API Handler

#### Drawbacks of direct client to m8s communication

* Causes problems as number of m8s increases
* When adding new m8s all the client applications would have to be modified accordingly.
* Increases latency and complexity on the UI side.

#### Feature

* Can Aggregate several m8s in one response
* Handles routing to internal m8s
* Helps in IAM
* Helps in Load Balancing
* Helps in Logging, Tracing

#### Cons

* Singele point of failure

#### Patterns

* Gateway Routing Pattern
* Gateway Aggregation Pattern
* Gateway Offloading Pattern

### BFF ( Backend for Frontends) 

* Seperate API Gateways usually based on clients

### Service Registry Pattern

* Used to know the IPs and ports of internal m8s
* k8s handles this internally.


### Asynchoronous Communication

* Event driven communication
* Helps in reducing coupling


#### Apache Kafka

* Open Source event streaming platforms
* Horizontally scalable , distributed and fault tolerant
* Topics, Partitions , Offsets and Replication Factor

**Use Cases**

* Log Aggregation
* Messaging btwn m8s
* Metrics - Operational data
* Stream Processing  
