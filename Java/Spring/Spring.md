# Spring

## How to setup spring boot project

* Go to spring initailizr <https://start.spring.io/> , giv the appropriate info about the project and download the project initial config.

* Open the project as a mvn project.

### Inital Components

* In ```pom.xml``` we have the

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>3.0.1</version>
  <relativePath/> <!-- lookup parent from repository -->
 </parent>
```

Each version of spring will its supported versions of libraries. It will be hard to manually find which is compatible so when we use this <em> spring-boot-starter-parent</em> as parent it will inherit the dependency management and hence we dont need to mention the version when using spring dependencies.

* Again , in the dependecies section of ```pom.xml``` we will have

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

This dependency is used to build RESTful applications.

Note: How the version is not specified as discussed in the previous point.

* In ```src/main/java/``` we will have class with the name projectnameAppilication which is annotated with 

```java
@SpringBootApplication
```

This is the starting point for our application
Also the above annotation is equal to the combination of 

```java
@Configuration
@EnableAutoConfiguration
@ComponentScan
```
