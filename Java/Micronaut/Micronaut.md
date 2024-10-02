# Micronaut

## Introduction

Micronaut is a JVM based f/w for building lightweight applications. It does
dependency injection at compile time instead of run-time unlike other f/ws.

## Advantages

* Less Startup time
* Less Resource consumption
* No reflection is required for dependency injection,
  so it is more faster startup and less memory requirements
* Works well for cloud-native applications since it supports different service
  discovery softwares like Eureka and Consul and other distributed tracing softwares

## Comparison with other frameworks

| Feature/Aspect               | Micronaut                                             | Spring Boot                                                     | Quarkus                                                   |
|------------------------------|-------------------------------------------------------|-----------------------------------------------------------------|-----------------------------------------------------------|
| **Architecture**             | Microservice-oriented, modular                        | Monolithic and microservice-oriented                            | Microservice-oriented, modular                            |
| **Startup Time**             | Very fast due to AOT compilation                      | Slower due to reflection and classpath scanning                 | Very fast due to build-time optimizations                 |
| **Memory Footprint**         | Low, due to compile-time DI and AOT                   | Higher, due to reflection and runtime features                  | Low, optimized for GraalVM and native images              |
| **Dependency Injection**     | Compile-time DI, no runtime reflection                | Runtime DI with reflection                                      | Compile-time DI, optimized for GraalVM                    |
| **Performance**              | High, with low latency and overhead                   | Good, but can be impacted by reflection                         | High, with optimizations for low latency                  |
| **Reactive Support**         | Built-in, using Project Reactor and RxJava            | Available through Spring WebFlux                                | Built-in, using Mutiny and Reactor                        |
| **Configuration Management** | Built-in support, YAML, and properties files          | Built-in support, YAML, properties, and advanced configurations | Built-in support, with hot reload capabilities            |
| **Cloud-Native Features**    | Built-in service discovery, distributed configuration | Extensive ecosystem with Spring Cloud                           | Integrated with Kubernetes and OpenShift                  |
| **Native Compilation**       | Supports GraalVM native images                        | Limited support, can use Spring Native                          | Strong support for GraalVM native images                  |
| **Tooling & Ecosystem**      | Growing, but not as extensive as Spring               | Mature, with a wide range of extensions                         | Growing, with focus on modern cloud environments          |
| **Testing**                  | Excellent support with minimal configuration          | Extensive testing support with many libraries                   | Good support, particularly for integration and unit tests |
| **Learning Curve**           | Moderate, due to modern paradigms                     | Steeper, due to its comprehensive ecosystem                     | Moderate, focused on modern practices                     |
| **Community & Support**      | Growing, but smaller than Spring                      | Large, well-established community                               | Growing, with strong backing from Red Hat                 |

## Dependency Injection

### `@Inject`

Used to autowire a bean similar `@Autowired` in spring f/w.

**Note:** All beans are of prototype scope by default.

### `@Singleton`

Used to create beans of scope singleton

### `@Primary`

Same as spring f/w , used to resolve ambiguity in-case multiple beans qualify.

### `@Requires`

Similar to `@Conditional`

## Building HTTP Server

```bash 
mn create-app <<project name>> -build maven
```

### Blocking

`@Controller` to mark a class as controller.

`@Get("/{name}")` for get mapping

`@Post(value = "/{name}", consumes = MediaType.TEXT_PLAIN)` for post mapping

`@Body` for request body

GET endpoint with path variable and query param 
```java
package example.micronaut;

import io.micronaut.http.annotation.Controller;
import io.micronaut.http.annotation.Get;
import io.micronaut.http.annotation.QueryValue;
import io.micronaut.http.annotation.PathVariable;

@Controller("/greet") // Base path for the controller
public class GreetingController {

    @Get("/{name}/age/{age}") // Path variables: name and age
    public String greet(@PathVariable String name, 
                        @PathVariable int age, 
                        @QueryValue(defaultValue = "en") String lang) {
        // Query parameter: lang (with a default value of "en")

        String greeting;
        switch (lang) {
            case "es":
                greeting = "Hola";
                break;
            case "fr":
                greeting = "Bonjour";
                break;
            default:
                greeting = "Hello";
        }

        return String.format("%s, %s! You are %d years old.", greeting, name, age);
    }
}
```


###  Custom Error Handling:

1. **Create a Custom Exception**:
   Define a custom exception that can be thrown in your service or controller when an error occurs.

2. **Implement an Exception Handler**:
   Create a class that implements `ExceptionHandler<T extends Throwable, R>` where `T` is the exception type to be handled and `R` is the return type (usually `HttpResponse`).

3. **Register the Handler**:
   Micronaut automatically detects and registers exception handlers based on the `@Singleton` annotation.


#### 1. Create a Custom Exception:

```java
package com.example.exception;

public class CustomNotFoundException extends RuntimeException {
    public CustomNotFoundException(String message) {
        super(message);
    }
}
```

#### 2. Create a Custom Exception Handler:

```java
package com.example.exception;

import io.micronaut.http.HttpRequest;
import io.micronaut.http.HttpResponse;
import io.micronaut.http.HttpStatus;
import io.micronaut.http.annotation.Produces;
import io.micronaut.http.server.exceptions.ExceptionHandler;

import javax.inject.Singleton;

@Produces
@Singleton
public class CustomNotFoundExceptionHandler implements ExceptionHandler<CustomNotFoundException, HttpResponse<?>> {

    @Override
    public HttpResponse<?> handle(HttpRequest request, CustomNotFoundException exception) {
        // You can create a custom response body if needed
        return HttpResponse.status(HttpStatus.NOT_FOUND)
                .body(new ErrorResponse("Resource Not Found", exception.getMessage()));
    }
    
    // A simple class for custom error response
    static class ErrorResponse {
        private final String error;
        private final String message;

        public ErrorResponse(String error, String message) {
            this.error = error;
            this.message = message;
        }

        public String getError() {
            return error;
        }

        public String getMessage() {
            return message;
        }
    }
}
```

#### 3. Throw the Custom Exception in Your Controller:

```java
package com.example.controller;

import com.example.exception.CustomNotFoundException;
import io.micronaut.http.annotation.Controller;
import io.micronaut.http.annotation.Get;

@Controller("/items")
public class ItemController {

    @Get("/{id}")
    public String getItem(String id) {
        // Simulating an error scenario
        if ("404".equals(id)) {
            throw new CustomNotFoundException("Item with id " + id + " not found.");
        }
        return "Item " + id;
    }
}
```
### Local Custom error handling

### Key Use Cases of `@Error` Annotation:
1. **Handle Specific Exceptions**: It allows handling of specific exceptions that occur within a controller method. For example, if a method throws a `CustomNotFoundException`, you can use `@@Error` to handle it and return a custom response.

2. **Fallback Mechanism**: It can act as a fallback mechanism to return a default response when an exception occurs, without propagating the error further.

3. **Local Error Handling**: The error handling defined with `@@Error` is local to the controller where it is defined. It will not be applied globally, making it useful when you want specific controllers to have their own error-handling logic.

### Example: Using `@Error` Annotation

Here is an example demonstrating how to use the `@@Error` annotation in a Micronaut controller.

```java
package com.example.controller;

import com.example.exception.CustomNotFoundException;
import io.micronaut.http.HttpResponse;
import io.micronaut.http.annotation.Controller;
import io.micronaut.http.annotation.Error;
import io.micronaut.http.annotation.Get;

@Controller("/items")
public class ItemController {

    // A regular controller method that can throw a custom exception
    @Get("/{id}")
    public String getItem(String id) {
        if ("404".equals(id)) {
            throw new CustomNotFoundException("Item with id " + id + " not found.");
        }
        return "Item " + id;
    }

    // Local error handler using the @@Error annotation to handle the custom exception
    @Error(exception = CustomNotFoundException.class)
    public HttpResponse<String> handleCustomNotFound(CustomNotFoundException exception) {
        return HttpResponse.notFound("Custom Error: " + exception.getMessage());
    }

    // Catch-all error handler for any other exceptions that may occur in this controller
    @Error
    public HttpResponse<String> handleGeneralError(Throwable error) {
        return HttpResponse.serverError("An unexpected error occurred: " + error.getMessage());
    }
}
```

### Explanation:
- The `getItem()` method simulates an error scenario where if the ID is `404`, a `CustomNotFoundException` is thrown.
- The method annotated with `@@Error` and `exception = CustomNotFoundException.class` will catch this specific exception and return a 404 error response with a custom message.
- The second `@@Error` method serves as a **generic fallback** for any other exceptions that may occur in the controller.

### Key Points:
- **Local Scope**: The `@@Error` annotation applies only to the controller in which it's defined. If you need global exception handling, you would use a global exception handler class.
- **Specific or Generic Handling**: You can define error handling for specific exception types or create a catch-all method for any uncaught exceptions.
- **Return Custom Responses**: Methods annotated with `@@Error` can return custom `HttpResponse` objects, allowing you to define the error status, headers, and body.


### Reactive

To make the application reactive just change return type to reactive components.

## Testing

### Checking if application is running
```java
import io.micronaut.runtime.EmbeddedApplication;
import io.micronaut.test.extensions.junit5.annotation.MicronautTest;
import org.junit.jupiter.api.Test;
import jakarta.inject.Inject;
import static org.junit.jupiter.api.Assertions.assertTrue;

@MicronautTest // This sets up the Micronaut environment for testing
class ApplicationTest {

    // Injecting the EmbeddedApplication to check if it's running
    @Inject
    EmbeddedApplication<?> application;

    @Test
    void testApplicationIsRunning() {
        // Assert that the application is running
        assertTrue(application.isRunning());
    }
}

```

## Serialization and deserialization

### `@Serdeable` in Micronaut

The `@Serdeable` annotation is used to indicate that a class can be serialized and deserialized using Micronaut's **serialization engine**. It is part of the **micronaut-serialization** module, which is an alternative to the more common **Jackson** library used for JSON (and other formats) serialization/deserialization.

### Usage of `@Serdeable`

Here’s how you can use `@Serdeable` in Micronaut:

1. **Annotating a Class for Serialization/Deserialization**:
   You can annotate a class with `@Serdeable` to mark it as eligible for automatic serialization/deserialization by Micronaut.

   ```java
   import io.micronaut.serde.annotation.Serdeable;

   @Serdeable
   public class Person {
       private String name;
       private int age;

       // Constructors, getters, setters, etc.
   }
   ```

2. **Serialization and Deserialization**:
  - **Serialization**: When you convert a Java object to a format like JSON.
  - **Deserialization**: When you convert JSON (or other formats) back into a Java object.

   With `@Serdeable`, Micronaut can automatically serialize and deserialize objects when needed, such as in HTTP requests and responses.

3. **Customizing Serdeable**:
   You can customize serialization behavior by using additional attributes in the `@Serdeable` annotation. For example:

   ```java
   @Serdeable(
       include = Serdeable.Include.NON_NULL // Only include non-null fields in serialization
   )
   public class Person {
       private String name;
       private int age;
   }
   ```

4. **Advantages of `@Serdeable` over Jackson**:
  - **Performance**: Micronaut’s serialization engine can be more efficient than Jackson for certain use cases.
  - **Modular**: You can choose to use it without depending on Jackson or other libraries.
  - **Native Image**: Works better with GraalVM native image support because it does not rely on runtime reflection.

<details>
<summary>Example of HTTP Controller using `@Serdeable`</summary>
import io.micronaut.http.annotation.*;
import io.micronaut.http.MediaType;
import io.micronaut.serde.annotation.Serdeable;

@Controller("/person")
public class PersonController {

    @Post(consumes = MediaType.APPLICATION_JSON, produces = MediaType.APPLICATION_JSON)
    public Person createPerson(@Body Person person) {
        return person; // Micronaut will automatically deserialize request and serialize the response
    }
}

@Serdeable
class Person {
private String name;
private int age;

    // getters and setters
}
</details>

## References

* chatgpt
* https://www.baeldung.com/micronaut