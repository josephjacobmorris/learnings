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


| Feature/Aspect           | Micronaut                               | Spring Boot                               | Quarkus                                  |
|--------------------------|-----------------------------------------|-------------------------------------------|------------------------------------------|
| **Architecture**         | Microservice-oriented, modular         | Monolithic and microservice-oriented      | Microservice-oriented, modular           |
| **Startup Time**         | Very fast due to AOT compilation        | Slower due to reflection and classpath scanning | Very fast due to build-time optimizations |
| **Memory Footprint**     | Low, due to compile-time DI and AOT     | Higher, due to reflection and runtime features | Low, optimized for GraalVM and native images |
| **Dependency Injection** | Compile-time DI, no runtime reflection  | Runtime DI with reflection                | Compile-time DI, optimized for GraalVM   |
| **Performance**          | High, with low latency and overhead     | Good, but can be impacted by reflection   | High, with optimizations for low latency |
| **Reactive Support**     | Built-in, using Project Reactor and RxJava | Available through Spring WebFlux          | Built-in, using Mutiny and Reactor       |
| **Configuration Management** | Built-in support, YAML, and properties files | Built-in support, YAML, properties, and advanced configurations | Built-in support, with hot reload capabilities |
| **Cloud-Native Features**| Built-in service discovery, distributed configuration | Extensive ecosystem with Spring Cloud   | Integrated with Kubernetes and OpenShift  |
| **Native Compilation**   | Supports GraalVM native images           | Limited support, can use Spring Native    | Strong support for GraalVM native images |
| **Tooling & Ecosystem**  | Growing, but not as extensive as Spring | Mature, with a wide range of extensions   | Growing, with focus on modern cloud environments |
| **Testing**              | Excellent support with minimal configuration | Extensive testing support with many libraries | Good support, particularly for integration and unit tests |
| **Learning Curve**       | Moderate, due to modern paradigms       | Steeper, due to its comprehensive ecosystem | Moderate, focused on modern practices   |
| **Community & Support**  | Growing, but smaller than Spring         | Large, well-established community          | Growing, with strong backing from Red Hat |


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

### Reactive

To make the application reactive just change return type to reactive components.

## References

* chatgpt
* https://www.baeldung.com/micronaut