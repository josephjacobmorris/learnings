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

## Syntax
**Dependencies for Spring Batch**
```kotlin
implementation("org.springframework.boot:spring-boot-starter-batch")

```
** Sample Spring Batch Configuration
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