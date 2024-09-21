# New Relic

## Introduction
Observability in microservices architecture is essential due to the following reasons:

1. **Complexity Management**: Microservices involve numerous interconnected services, making it difficult to trace and diagnose issues. Observability provides visibility into how services interact.

2. **Real-time Monitoring**: It enables continuous monitoring of service health and performance, allowing for immediate detection and response to anomalies.

3. **Improved Debugging**: Detailed logs, metrics, and traces help in identifying the root cause of failures or performance bottlenecks across distributed services.

4. **Enhanced Reliability**: By proactively detecting issues, observability ensures services remain reliable, reducing downtime and maintaining user trust.

5. **Scalability**: As microservices scale, observability tools allow tracking of system behavior under load, ensuring consistent performance.

6. **Faster Incident Response**: With observability, teams can quickly identify and resolve issues, reducing Mean Time to Resolution (MTTR).

7. **Data-driven Decision Making**: Observability provides actionable insights that help in optimizing services and improving system architecture.

### Monitoring helps us answer 3 questions
* Is the service on?
* Is the service functioning as expected?
* Is the service performing well?

### Important metrics to measure observability suc
* MTTD - Mean time to detect - the time between the start of an issue and the time team becomes aware of it
* MTTR - Mean time to resolution - the time between the start of an issue and the time team takes to fix it or system becomes normal

### Methods of Observability
Typical m8s have 3 layers
* UI layer -observed via Core web vitals such as Percieved page load, Percieved page responsiveness, Percieved Stability)
* Service layer - observed by RED(Rate TPS, Error, Duration or latency) method
* Infrastructure layer - observed by USE (utilization, Saturation , Errors) method

### Types of Telemetry data
Telemetry data in observability is broadly categorized into 4 main types:

#### 1. **Logs**
- **Definition**: Logs are time-stamped, discrete records of events that occur within a system.
- **Usage**: Logs capture detailed information about specific events, such as errors, state changes, or user actions. They are crucial for debugging and understanding the sequence of operations.
- **Example**: An error log entry when a service fails to connect to a database.

#### 2. **Metrics**
- **Definition**: Metrics are numerical measurements that represent the state or performance of a system over time.
- **Usage**: Metrics are used for real-time monitoring, tracking trends, and alerting. They provide insights into resource utilization, latency, error rates, and other performance indicators.
- **Example**: CPU utilization percentage, request count per second, or memory usage.

#### 3. **Traces**
- **Definition**: Traces represent the journey of a request or transaction as it propagates through different services in a distributed system.
- **Usage**: Traces help in understanding the flow of requests across services, identifying bottlenecks, and diagnosing performance issues in microservices architectures.
- **Example**: A trace showing the path of an API call through multiple microservices, with details of time spent in each service.

#### 4. **Events**
- **Definition**: Events are significant state changes or conditions within a system.
- **Usage**: Events track specific occurrences that may require attention or action, such as scaling operations, deployment changes, or threshold breaches.
- **Example**: An event notification for a new service deployment or a scale-up operation.

> NOTE: Remember MELT as acronym

### Observality Platform
An observability platform is a comprehensive tool or suite of tools designed to provide visibility into the performance, health, and behavior of systems, particularly in complex environments like microservices architectures. It collects, processes, and analyzes telemetry data (logs, metrics, traces, etc.) to enable monitoring, troubleshooting, and optimization of services.

### Methods of Collecting Telemetry Data

1. **Agent-based Collection**
    - **Description**: Agents are lightweight software components installed on each service or host to collect telemetry data. They gather logs, metrics, and traces locally and forward them to the observability platform.
    - **Pros**:
        - **High Granularity**: Agents provide detailed, fine-grained data collection specific to each service or host.
        - **Customization**: Agents can be configured to collect specific types of data, offering flexibility.
        - **Low Latency**: Since agents run locally, data collection and transmission are typically fast.
    - **Cons**:
        - **Resource Overhead**: Agents consume system resources (CPU, memory), potentially impacting the performance of the host services.
        - **Management Complexity**: Maintaining and updating agents across multiple services can be challenging.
        - **Compatibility Issues**: Agents may need to be tailored to specific environments, leading to compatibility challenges.

2. **Sidecar Pattern**
    - **Description**: In microservices architectures, sidecar containers are deployed alongside the main service container. These sidecars handle tasks like telemetry data collection, networking, and monitoring without interfering with the primary service logic.
    - **Pros**:
        - **Isolation**: The sidecar pattern separates observability concerns from the main application, reducing complexity in the service code.
        - **Scalability**: As new services are deployed, sidecars can be easily added, ensuring consistent observability across the system.
        - **Portability**: Sidecars are containerized and can be deployed across different environments (e.g., Kubernetes).
    - **Cons**:
        - **Increased Resource Consumption**: Running additional sidecar containers increases the overall resource usage.
        - **Operational Complexity**: Managing sidecars for every service can add operational overhead, especially in large-scale deployments.
        - **Latency**: Introducing sidecars may slightly increase network latency due to the extra hop between the service and sidecar.

3. **Library Instrumentation**
    - **Description**: Libraries or SDKs are integrated directly into the application code to collect telemetry data. Developers instrument their code with these libraries to generate logs, metrics, and traces.
    - **Pros**:
        - **High Customization**: Developers have full control over what data is collected, allowing for tailored observability specific to business logic.
        - **Minimal Overhead**: Since the collection is embedded in the code, there’s no need for additional agents or sidecars.
        - **Direct Access**: Libraries can access internal application states directly, enabling precise data collection.
    - **Cons**:
        - **Developer Burden**: Instrumentation requires developers to modify code, adding to development effort and complexity.
        - **Code Coupling**: The application becomes tightly coupled with specific observability libraries, which can make future changes or migrations challenging.
        - **Performance Impact**: Poorly designed instrumentation can introduce performance overhead and slow down the application.

4. **Service Mesh**
    - **Description**: A service mesh is a dedicated infrastructure layer that handles communication between microservices. It can also capture telemetry data related to service-to-service communication, such as latency, errors, and traffic.
    - **Pros**:
        - **Transparent Operation**: The service mesh operates at the network level, requiring no changes to application code or deployment processes.
        - **Centralized Control**: It provides a consistent, centralized way to manage observability, security, and traffic policies across all services.
        - **Advanced Features**: Service meshes often come with built-in observability features, such as distributed tracing and traffic monitoring.
    - **Cons**:
        - **Complexity**: Deploying and managing a service mesh introduces additional complexity to the system architecture.
        - **Resource Overhead**: Running a service mesh consumes additional resources, particularly in terms of CPU and memory.
        - **Latency**: The service mesh introduces some additional network latency, which may be significant in high-performance applications.

5. **Polling and Scraping**
    - **Description**: This method involves periodically querying or scraping services for metrics or logs, typically using tools like Prometheus that pull data from endpoints exposed by the services.
    - **Pros**:
        - **Low Intrusion**: Polling allows data collection without modifying the application or deploying additional components (like agents or sidecars).
        - **Simplicity**: This method is easy to set up and manage, especially for collecting metrics at regular intervals.
        - **Centralized Control**: A central system (like Prometheus) manages all data collection, simplifying configuration.
    - **Cons**:
        - **Limited Granularity**: Polling intervals may miss short-lived events or rapid fluctuations in performance.
        - **Increased Load**: Frequent polling can put a load on the services being monitored, potentially affecting their performance.
        - **Latency**: Data may not be available in real-time due to the periodic nature of polling, leading to delays in detecting issues.

Each method of collecting telemetry data has its own set of advantages and trade-offs, and the choice often depends on the specific requirements and constraints of the system being monitored. In practice, a combination of these methods is often used to achieve comprehensive observability.

## Basics of newrelic
### Setup of newrelic cli
Look up steps corresponding to os from [here](https://github.com/newrelic/newrelic-cli?tab=readme-ov-file#installation--upgrades]
https://github.com/newrelic/newrelic-cli)

To add new profile to newrelic-cli
```bash
sudo newrelic profile add --profile LearnNewRelicJose --apiKey <paste api key here> -r <eu/us> --accountId <paste account id here>
```

### 1. Entities
Entities in New Relic represent the fundamental units of monitoring data. They can be various types of components within your infrastructure or applications. For example, entities might include:
- **Applications:** Monitored apps, including web applications, mobile apps, and server-side applications.
- **Hosts:** Individual servers or instances running your applications.
- **Services:** Specific services or microservices that are part of your architecture.
- **Databases:** Database instances or clusters.

Entities are central to New Relic’s data model because they are the objects for which performance metrics and other telemetry data are collected and displayed.

### 2. Tags
Tags are key-value pairs that you can assign to entities in New Relic. They help in categorizing and organizing data for easier management and analysis. Tags can be used to:
- **Group Entities:** For example, you might tag entities with their environment (e.g., `environment:production`) or their role (e.g., `role:frontend`).
- **Filter Data:** Tags allow you to filter and drill down into specific sets of data. For example, you might want to see metrics only for entities tagged as `region:us-east`.
- **Create Custom Dashboards and Alerts:** Tags can be used to create customized dashboards and alert policies based on specific criteria relevant to your business.

Apdex, or Application Performance Index, is a metric used to measure user satisfaction with application performance. In New Relic, Apdex is a key performance indicator that helps you understand how well your application is meeting user expectations regarding response times.

### What is Apdex?

Apdex is a standardized way to quantify user satisfaction based on the response times of an application. It provides a single score that reflects the percentage of user interactions that meet performance targets.

#### How is Apdex Calculated?

Apdex is calculated using the following formula:

\[ \text{Apdex} = \frac{S + \frac{T}{2}}{N} \]

Where:
- \( S \) is the number of satisfactory interactions.
- \( T \) is the number of tolerating interactions.
- \( N \) is the total number of interactions.

#### Detailed Breakdown

1. **Satisfactory Interactions (S):** These are the interactions where the response time is less than or equal to the defined threshold for satisfactory performance (often referred to as \( T_s \)). This is the optimal performance where users are generally satisfied.

2. **Tolerating Interactions (T):** These interactions have response times that exceed the satisfactory threshold but are still within a tolerable range (often referred to as \( T_t \)). Users may experience some delays but are still able to use the application without major issues.

3. **Total Interactions (N):** This is the total number of interactions or transactions considered for Apdex calculation.

#### Steps to Calculate Apdex

1. **Define Thresholds:** Set the thresholds for satisfactory and tolerating performance. For example:
   - **Satisfactory Threshold (T_s):** Time within which users are highly satisfied.
   - **Tolerating Threshold (T_t):** Time within which users are still able to tolerate some delays but are not fully satisfied.

2. **Collect Data:** Gather data on response times for interactions or transactions in your application.

3. **Categorize Interactions:**
   - Count how many interactions are within the satisfactory threshold.
   - Count how many interactions fall between the satisfactory and tolerating thresholds.

4. **Apply Formula:** Use the Apdex formula to calculate the Apdex score based on the counts from the previous step.

### Apdex Score Interpretation

- **Apdex Score Range:** The Apdex score ranges from 0 to 1.
   - **1:** Perfect Apdex score indicates that all interactions are satisfactory.
   - **0:** A score of 0 indicates that none of the interactions meet the satisfactory threshold.

- **Score Interpretation:** Higher Apdex scores indicate better performance and higher user satisfaction. Conversely, lower scores indicate performance issues and lower user satisfaction.

## NRQL Syntax
**Query Your Data** is a useful widget to create queries or view various metrics raw data
### Transaction

```
SELECT * FROM transaction facet host  WHERE transactionType in ('web',Web)
```
Now the above query runs on entire telemetry data .To time slice it we can use SINCE and/or UNTIL
```
SELECT * FROM transaction facet host  WHERE transactionType in ('web',Web) SINCE 1 day ago UNTIL 1 minute ago
```

Now lets say we want to see the for every hour we can use TIMESERIES for that
```
SELECT * FROM transaction facet host  WHERE transactionType in ('web',Web) SINCE 1 day ago UNTIL 1 minute ago TIMESERIES 1 HOUR
```

## References
* chatgpt
* udemy - Metric and Log Collection with Agents, New Relic Query Language (NRQL), Alerts and Incidents, and New Relic CLI
