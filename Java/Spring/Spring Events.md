# Spring Events

## Introduction

Certainly! Here's a summarized version of the important points with minimal code examples:

### 1. Basics of Spring Events:
- Events are a powerful but often overlooked feature in Spring.
- Event publishing is done through the ApplicationContext.

### 2. Creating a Custom Event:
- Prior to Spring 4.2, custom events extended `ApplicationEvent`.
- Event publishers inject `ApplicationEventPublisher`.
- Event listeners implement `ApplicationListener`.

```java
  // Custom event class
public class CustomSpringEvent extends ApplicationEvent {
public CustomSpringEvent(Object source, String message) {
super(source);
// constructor logic
}
}

// Event publisher
@Component
public class CustomSpringEventPublisher {
@Autowired
private ApplicationEventPublisher applicationEventPublisher;

    public void publishCustomEvent(final String message) {
        CustomSpringEvent customSpringEvent = new CustomSpringEvent(this, message);
        applicationEventPublisher.publishEvent(customSpringEvent);
    }
}

// Event listener
@Component
public class CustomSpringEventListener {
@EventListener
public void handleCustomEvent(CustomSpringEvent event) {
// event handling logic
}
```

### 3. Asynchronous Events:
- Asynchronous event handling can be configured using `SimpleAsyncTaskExecutor`.

 ```java
// Configuration for asynchronous events
@Configuration
public class AsynchronousSpringEventsConfig {
@Bean
public ApplicationEventMulticaster simpleApplicationEventMulticaster() {
SimpleApplicationEventMulticaster eventMulticaster = new SimpleApplicationEventMulticaster();
eventMulticaster.setTaskExecutor(new SimpleAsyncTaskExecutor());
return eventMulticaster;
}
}

   ```

### 4. Existing Framework Events:
- Spring provides default events like `ContextRefreshedEvent`.

### 5. Annotation-Driven Event Listener:
- Starting from Spring 4.2, use `@EventListener` on any public method.
- Method signature declares the event type it consumes.

```java
// Annotation-driven event listener
@Component
public class AnnotationDrivenEventListener {
@EventListener
public void handleContextStart(ContextStartedEvent event) {
// event handling logic
}
}
 ```

### 6. Generics Support:
- Generic events allow flexibility without extending `ApplicationEvent`.
- Annotation-driven listener with SpEL condition:

```java
// Generic event class
public class GenericSpringEvent<T> {
private T what;
private boolean success;

    public GenericSpringEvent(T what, boolean success) {
        // constructor logic
    }
}

// Generic event listener
@Component
public class GenericSpringEventListener {
@EventListener(condition = "#event.success")
public void handleSuccessful(GenericSpringEvent<String> event) {
// event handling logic
}
}
   ```

### 7. Transaction-Bound Events:
- Use `@TransactionalEventListener` for transaction-bound events.
- Specify the transaction phase (e.g., `BEFORE_COMMIT`).

 ```java
 // Transactional event listener
@TransactionalEventListener(phase = TransactionPhase.BEFORE_COMMIT)
public void handleCustom(CustomSpringEvent event) {
    // event handling logic
}
```

### 8. Conclusion:
- Spring events provide a flexible and extensible mechanism for communication within an application.
- Custom events, asynchronous handling, and transaction-bound events enhance event-driven architecture in Spring.

## References
* https://www.baeldung.com/spring-events
* Chatgpt