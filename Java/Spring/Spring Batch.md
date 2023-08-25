# Spring Batch

## Introduction

Spring Batch is a powerful framework for building and executing batch processing applications in the Java ecosystem. Designed to simplify the development of robust, scalable, and maintainable batch processes, it offers a comprehensive set of features.

At its core, Spring Batch provides a structured approach to processing large volumes of data by breaking tasks into smaller, manageable chunks. It supports batch jobs composed of steps, each of which can involve reading, processing, and writing data. This architecture promotes modularity and reusability.

Spring Batch offers essential features like error handling, transaction management, and job scheduling. It integrates seamlessly with various data sources and destinations, making it versatile for ETL (Extract, Transform, Load) processes, data migrations, and more.

Additionally, Spring Batch includes tools for monitoring and managing batch jobs, ensuring operational excellence. It's an excellent choice for developers looking to streamline batch processing workflows and maintain code quality in enterprise-level applications.

## Architecture
Spring Batch is a comprehensive framework for building and executing batch processing applications in Java. Its architecture comprises various components that work together to handle batch processing efficiently. Here are the key components in the Spring Batch architecture:

1. **Job**: A job is the highest-level component in Spring Batch. It represents a complete batch process and encapsulates a series of steps that need to be executed. Jobs are defined with configuration and are typically initiated by a scheduler or triggered manually.

2. **Step**: A step is a single, self-contained unit of work within a job. It represents a specific action in the batch process, such as reading data, processing it, and writing the results. Steps are organized sequentially within a job, and each step can have its own set of processing logic, error handling, and transaction boundaries.

3. **Item**: An item is a single piece of data processed within a step. It could be a record from a database, a line from a file, or any other data unit that the batch process operates on. Spring Batch provides support for processing items in chunks, which can significantly improve processing performance.

4. **Reader**: A reader is responsible for reading items from a data source (e.g., database, file, message queue) and providing them to a step for processing. Spring Batch offers various readers tailored for different data sources.

5. **Processor**: A processor is an optional component that performs business logic on each item read by the reader. It allows you to transform, filter, or enrich the data before it's written to the output.

6. **Writer**: A writer is responsible for writing items to a data destination, typically after processing by the reader and optionally the processor. Spring Batch provides various writers to support different output destinations (e.g., database, file, messaging system).

7. **Listener**: Listeners are callback mechanisms that allow you to hook into various points in the batch processing lifecycle, such as before or after a step is executed, when a job is completed, or when an item is read, processed, or written. Listeners are useful for customizing and monitoring batch processes.

8. **Job Repository**: The job repository is a database or storage mechanism that Spring Batch uses to store metadata about job executions. This includes information about job instances, step executions, and job parameters. It helps maintain the state of batch jobs, enabling features like restartability and monitoring.

9. **JobLauncher**: The JobLauncher is responsible for starting job executions. It takes a job definition and job parameters and initiates the execution of a job. Spring Batch provides different implementations of the JobLauncher, including asynchronous and remote launchers.

10. **JobScheduler**: While not part of the core Spring Batch framework, a job scheduler (e.g., Quartz, cron) is often used to automate and schedule the execution of batch jobs at specific intervals or times.

These components work together to provide a structured and scalable framework for developing and executing batch processing applications in Spring Batch. Developers can configure and customize these components to meet the specific requirements of their batch processing workflows.

### Tasklet && Chunk Oriented Processing
In Spring Batch, "tasklet" and "chunk-oriented processing" are two approaches used for implementing the processing logic within a step of a batch job. They serve different purposes and have distinct characteristics:

1. **Tasklet**:

    - **Purpose**: A tasklet is a simple, single-operation component that allows you to execute custom logic within a step. It is suitable for scenarios where the processing logic can be performed in a single, typically short-lived, operation.

    - **Characteristics**:
        - Tasklets are ideal for non-repeatable, one-time tasks, such as sending notifications, generating reports, or invoking external processes.
        - They are implemented by implementing the `Tasklet` interface and overriding the `execute` method with the custom logic.
        - Tasklets are transactional by default. Spring Batch ensures that if an exception occurs during the tasklet's execution, the entire step can be rolled back.
        - Tasklets are suitable for tasks that do not involve reading and writing a large number of items.

2. **Chunk-Oriented Processing**:

    - **Purpose**: Chunk-oriented processing is a pattern used for batch jobs that involve processing large volumes of data, typically in chunks. It's designed for scenarios where data is read in chunks, processed in parallel, and then written in chunks.

    - **Characteristics**:
        - Chunk-oriented processing is well-suited for ETL (Extract, Transform, Load) scenarios and similar batch processes where data needs to be read from a source, processed in batches, and written to a destination.
        - It involves three key components: the **reader**, the **processor**, and the **writer**. Items are read one by one or in chunks from the input source by the reader, processed (optionally) by the processor, and then written to the output by the writer.
        - Spring Batch allows you to configure the chunk size, which defines the number of items processed together as a batch.
        - Chunk-oriented processing is designed for high throughput and efficient use of resources. It can handle large datasets and is suitable for repeatable and restartable batch jobs.

In summary, tasklets are best for small, non-repeating tasks within a step, while chunk-oriented processing is ideal for batch jobs that involve reading, processing, and writing data in chunks. The choice between them depends on the specific requirements and characteristics of the batch processing task you are implementing.

### Job Instance & Job Execution & Job Execution Context

In Spring Batch, when you run a batch job, it goes through several stages and involves various components to manage its execution. To understand the concepts of Job Instance, Job Execution, and Job Execution Context, let's break them down:

1. **Job Instance**:

   - **Definition**: A Job Instance represents a specific run or occurrence of a job. It's an instantiation of a job definition with a unique identifier.

   - **Characteristics**:
      - Each time you run a job, a new Job Instance is created, even if you're using the same job definition. Job Instances are meant to track the individual runs of a job.
      - Job Instances are identified by a combination of the job's name and a set of parameters. If you run the same job with different parameters (e.g., processing different input files), each run will result in a new Job Instance.
      - Job Instances are useful for tracking and managing the execution history of a job. You can query them to see when a job was executed, with what parameters, and whether it succeeded or failed.

2. **Job Execution**:

   - **Definition**: Job Execution is a specific run of a Job Instance. It represents a single execution attempt of a job, which includes one or more steps.

   - **Characteristics**:
      - When you start a job, a new Job Execution is created for the corresponding Job Instance. It records the start time, end time, status (e.g., completed, failed), and other metadata related to that specific run.
      - Job Executions can be uniquely identified by an ID within the context of a Job Instance.
      - Spring Batch keeps track of Job Executions to provide features like restartability (resuming a job from where it left off) and monitoring.
      - A Job Execution contains information about each step's execution within the job.

3. **Job Execution Context**:

   - **Definition**: Job Execution Context is a data storage mechanism provided by Spring Batch. It allows you to store and share data across the steps within a single Job Execution.

   - **Characteristics**:
      - Job Execution Context is implemented as a key-value store and can hold both simple and complex objects.
      - You can use the Job Execution Context to pass data or state information between steps. For example, you might store some state information in one step and retrieve it in a subsequent step.
      - It's particularly useful when you have a sequence of steps in your job, and you need to share information or context between them.
      - Job Execution Context is local to a specific Job Execution. Once the job completes, the context is typically discarded.

In summary, a Job Instance represents a single run of a job, Job Execution represents a specific execution attempt within that run, and the Job Execution Context allows for data sharing between steps within a Job Execution. These concepts are essential for managing and understanding the execution of batch jobs in Spring Batch.
### Task Execution & Task Execution Context
Task Execution and Task Execution Context are concepts related to the execution of individual tasks or units of work within a batch processing framework like Spring Batch. They help manage and store information related to the execution of tasks. Here's an explanation of these concepts:

1. **Task Execution**:

    - **Definition**: Task Execution refers to the execution of a single unit of work within a batch process. In the context of Spring Batch, a task can be a step within a job or any other discrete operation that needs to be executed.

    - **Characteristics**:
        - Task Execution is typically a short-lived operation that performs a specific task, such as reading a file, processing data, or writing to a database.
        - It is a fundamental building block in batch processing, and multiple task executions can be combined to create a batch job.
        - Task Executions can run sequentially or in parallel, depending on the design of the batch job.
        - Each Task Execution may have its own context or data associated with it, depending on the requirements of the task.

2. **Task Execution Context**:

    - **Definition**: Task Execution Context is a storage mechanism provided by batch processing frameworks like Spring Batch to manage and store data related to the execution of a task. It allows you to pass information between various stages of a task execution.

    - **Characteristics**:
        - Task Execution Context is implemented as a key-value store, similar to the Job Execution Context but scoped to a single task execution.
        - It is particularly useful when you need to share data or state information between different phases of a task, such as between the reader, processor, and writer in a chunk-oriented step.
        - Task Execution Context data is local to the scope of a task execution. Once the task execution completes, the context is typically discarded.
        - Common use cases for Task Execution Context include passing information like job parameters, statistics, error flags, or any other context-specific data between the components of a task.

In summary, Task Execution represents the execution of a specific unit of work within a batch job, while Task Execution Context is a storage mechanism that allows you to manage and pass data within the scope of that task execution. These concepts are crucial for orchestrating complex batch processes and ensuring that data and state information are managed effectively during batch job execution.

### Role of Job Parameters
Job parameters play a crucial role in customizing and making each run of a Spring Batch job unique. They allow you to pass runtime information or configuration values to your batch job, making it flexible and adaptable to different scenarios. Here's an explanation of the role of job parameters and how to make them unique for each run:

**Role of Job Parameters**:

1. **Customization**: Job parameters enable you to customize the behavior of your batch job without modifying its core logic. You can adjust parameters such as input file paths, processing thresholds, date ranges, or any other values that affect how the job operates.

2. **Uniqueness**: By including unique parameters for each job run, you can ensure that each execution is distinct. This is especially important for scenarios where you need to process different data sources, apply different filters, or generate unique outputs.

3. **Restartability**: Job parameters are essential for making batch jobs restartable. When a job fails and needs to be restarted, the original parameters are reused to ensure the job resumes from where it left off.

**How to Make Job Parameters Unique for Each Run**:

To make job parameters unique for each run, follow these steps:

1. **Define Job Parameters**:

   In your Spring Batch job configuration, define the job parameters you need. You can use the `JobParameters` class to encapsulate these parameters. For example:

    ```java
    @Bean
    public Job job() {
        return jobBuilderFactory.get("myJob")
            .incrementer(new RunIdIncrementer()) // This ensures uniqueness by adding a run id
            .start(myStep())
            .build();
    }
    ```

2. **Use an Incrementer**:

   To ensure uniqueness for each run, use an "incrementer." Spring Batch provides the `RunIdIncrementer`, which automatically increments the run id for each job execution, making the job parameters unique:

    ```java
    .incrementer(new RunIdIncrementer())
    ```

3. **Pass Parameters at Runtime**:

   When launching the batch job, you need to pass the job parameters as command-line arguments or through a scheduler. For example, if you're using the Spring Boot command-line runner, you can pass parameters like this:

    ```shell
    java -jar my-batch-job.jar inputFilePath=/path/to/input-data.csv date=2023-08-25
    ```

4. **Access Parameters in Your Job**:

   In your job configuration and job components (e.g., readers, processors, writers), you can access the job parameters using the `JobParameters` object. For example, you can inject it into your step and use it within your reader:

    ```java
    @StepScope
    @Bean
    public FlatFileItemReader<MyData> reader(@Value("#{jobParameters['inputFilePath']}") String inputFilePath) {
        // Use the inputFilePath parameter to configure your reader
        // ...
    }
    ```

With these steps, you ensure that job parameters are unique for each run of your batch job, making it highly adaptable and restartable. Each time you launch the job with different parameters, it creates a new job instance with a unique run id, preserving the distinctiveness of each execution.

## Syntax
**Dependencies for Spring Batch**
```kotlin
implementation("org.springframework.boot:spring-boot-starter-batch")

```
**Sample Spring Batch Configuration**
```java
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.configuration.annotation.EnableBatchProcessing;
import org.springframework.batch.core.configuration.annotation.JobBuilderFactory;
import org.springframework.batch.core.configuration.annotation.StepBuilderFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableBatchProcessing
public class BatchConfig {

    @Autowired
    private JobBuilderFactory jobBuilderFactory;

    @Autowired
    private StepBuilderFactory stepBuilderFactory;

    @Bean
    public Job sampleJob() {
        return jobBuilderFactory.get("sampleJob")
                .start(taskletStep())
                .next(chunkStep())
                .build();
    }

    @Bean
    public Step taskletStep() {
        return stepBuilderFactory.get("taskletStep")
                .tasklet(new SampleTasklet())
                .build();
    }

    @Bean
    public Step chunkStep() {
        return stepBuilderFactory.get("chunkStep")
                .<String, String>chunk(2) // Chunk size of 2
                .reader(new SampleItemReader())
                .processor(new SampleItemProcessor())
                .writer(new SampleItemWriter())
                .build();
    }
}

```

**Sample Tasklet Implementation**
```java
import org.springframework.batch.core.StepContribution;
import org.springframework.batch.core.scope.context.ChunkContext;
import org.springframework.batch.core.step.tasklet.Tasklet;
import org.springframework.batch.repeat.RepeatStatus;

public class SampleTasklet implements Tasklet {
    @Override
    public RepeatStatus execute(StepContribution contribution, ChunkContext chunkContext) throws Exception {
        // Your tasklet logic here
        System.out.println("Executing tasklet step...");
        return RepeatStatus.FINISHED;
    }
}

```

**Sample Item Reader, Processor & Item Writer**
```java
import org.springframework.batch.item.ItemProcessor;
import org.springframework.batch.item.ItemReader;
import org.springframework.batch.item.ItemWriter;
import java.util.List;

public class SampleItemReader implements ItemReader<String> {
    // Implement the reader logic to read data (e.g., from a list)
}

public class SampleItemProcessor implements ItemProcessor<String, String> {
    // Implement the processor logic to process the data
}

public class SampleItemWriter implements ItemWriter<String> {
    // Implement the writer logic to write processed data (e.g., to console)
}
```
## Reference
* Chatgpt