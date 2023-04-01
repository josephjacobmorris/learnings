# Micronaut

## Introduction

Micronaut is a JVM based f/w for building lightweight applications. It does
dependency injection at compile time instead of run-time unlike other f/ws.

## Advantages

* No reflection is required for dependency injection, 
so it is more faster startup and less memory requirements
* Works well for cloud-native applications since it supports different service
discovery softwares like Eureka and Consul and other distributed tracing softwares

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

* https://www.baeldung.com/micronaut