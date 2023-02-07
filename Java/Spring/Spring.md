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

Each version of spring will its supported versions of libraries. It will be hard to manually find which is compatible so when we use this <em>spring-boot-starter-parent</em> as parent it will inherit the dependency management and hence we dont need to mention the version when using spring dependencies.

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

## Writing a controller for mvc

```java
import com.example.spring5webapp.repositories.BookRepository;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class BookController
{
    private final BookRepository bookRepository;

    public BookController(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }

    @RequestMapping("/books")
    public String books(Model model){
        model.addAttribute("books", bookRepository.findAll());
        return "books/list";
    }
}
```

```@Controller``` annotation will mark the class as ```@Component``` and tells the spring it is function as a controller.

```@RequestMapping("/books")``` will map web request to methods

In the above code ```books/list``` is the path of the html file , by default it checks in the ```resources\templates``` folder

## Thymeleaf

For view we use Thymeleaf

### Dependency

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<table>
    <tr>
        <th>ID</th>
        <th>TITLE</th>
        <th>ISBN</th>
    </tr>

    <tr th:each="book :${books}">
        <td th:text="${book.id}">BookId</td>
        <td th:text="${book.title}">BookName</td>
        <td th:text="${book.isbn}">BookISBN</td>
    </tr>
</table>
</body>
</html>
```

## Spring Configuration

### Spring Stereotype Configuration

Configuration in Spring is done using the sterotypes ```@Component``` , ```@Controller``` , ```@Respository```, ```@Service```  and ```@RestController```.

```@RestController``` is a convenience annotation for ```@Controller``` and  ```@ResponseBody```.

### Using Configuration classes

The argument against using too many spring components is that spring uses reflection to do the component scan and java reflection is very slow. So instead we could use ```@Configuration``` classes to define the  beans using methods annonated with ```@Bean``` annotation. The method name is considered as the bean name by default.
If the bean depends on another bean , then we need to add the dependant property as a paramter to the method. 

```@Primary``` and ```@Profile```
annotations can be applied to the method in the configuration class.

### XML Configuration

Adding configuration as xml file can be done via 

```java
@ImportResource("classpath:<xml filename>")
@Configuration
public class GreetingServiceConfig {
    ...    
}
```

and defining the xml file in the resources folder.


## Scope of Spring Beans

* Singleton - (default) Only one instance of the bean is created in the IoC container

* Prototype - A new instance is created each time the bean is requested

* Request - A single instance per http request. Only valid in the context of a web-aware Spring
ApplicationContext

* Session - A single instance per http session. Only valid in the context of a web-aware Spring
ApplicationContext.

* Global Session - A single instance per global session. Typically only used in a Portlet context.
Only valid in the context of a web-aware Spring ApplicationContext. 

* Application - bean is scoped to the lifecycle of a ServletContext. Only valid in the context of a web aware application

* Web Socket - Scopes a single bean definition to the lifecycle of a WebSocket. Only valid in the context of a web-aware Spring ApplicationContext

* Custom - Spring Scopes are extensible, and you can define your own scope by implementing Spring’s “Scope” interface.You cannot override the built in Singleton and Prototype Scopes.

The ```@Scope``` annotation denotes the scope of the bean.

## External Properties in Spring F/W

### Using ```@PropertySource```

```java
public class FakeDataSource {
    private final String username;

    private final String password;

    private final String jdbcUrl;

    public FakeDataSource(String username, String password, String jdbcUrl) {
        this.username = username;
        this.password = password;
        this.jdbcUrl = jdbcUrl;
    }
}
```

```java
import com.example.datasources.FakeDataSource;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;

@PropertySource("classpath:datasource.properties")
@Configuration
public class DataSourceConfiguration {
    @Bean
    FakeDataSource fakeDataSource(@Value("${ds.username}") String username, @Value("${ds.password}") String password, @Value("${ds.jdbcUrl}") String jdbcUrl) {
        return new FakeDataSource(username, password, jdbcUrl);
    }
}
```

```@PropertySource``` can only be used in conjunction with  ```@Configuration```. If we add the required properties to the application properties file then we dont have to use ```@PropertySource``` annotation anymore

Property values can be overridden by using program arguments or using Environment variables.We can also set profile specific application properties properties by naming the file as application-\<profile> properties. 


```@ConfigurationProperties``` annotation will search for the properties in the properties file and when the name matches the variable name then it will be automatically bound.

We also have a option to do constructor binding using the ```@ConstructorBinding``` annotation in the constructor of the bean class and in the Configuration class use ```@EnableConfigurationProperties``` annotation

## Testing in Spring

There are three types of tests Unit tests, Integration tests and Functional tests (e2e tests)

Code Under Test - This is the code (or application) you are
testing

Test Fixture - "A test fixture is a fixed state of a set of objects
used as a baseline for running tests. The purpose of a test
fixture is to ensure that there is a well known and fixed
environment in which tests are run so that results are
repeatable." - JUnit Doc

TDD - When test cases are written first and code is written to fix them

### Test Scope Dependencies

spring-boot-starter-test dependencies provides

* JUnit
* Spring Boot Test
* AssertJ
* Hamcrest
* Mockito
* JSONassert
* JSONPath

### Spring Boot Test Annotations

|Annotations| Usage|
|:----------:|:----------:|
|```@RunWith(SpringRunner.class)``` | When need to use spring features like autowiring , mockbean etc|
|```@SpringBootTest``` |useful when we need to bootstrap the entire container.|
|```@TestConfiguraiton```| used to mark test configuration class|
| ```@MockBean``` | gives a mockito mock value for variable|
|```@SpyBean``` ||
|```@JsonTest```| Annotation for a JSON test that focuses only on JSON serialization.|
|```@WebMvcTest```| Annotation that can be used for a Spring MVC test that focuses only on Spring MVC components.|
| ```@DataJpaTest``` | provides some standard setup needed for testing the persistence layer such as configuring H2, an in-memory database,setting Hibernate, Spring Data, and the DataSource, performing an @EntityScan, and turning on SQL logging |
|```@JdbcTest```| Similar to ```@DataJpaTest``` but does not configure the entity manager|
|```@DataMongoTest```|Configures an embedded MongoDB for testing|
|```@RestClientTest```||
|```@AutoConfigureRestDocks```||
|```@BootStrapWith```||
|```@ContextConfiguration```||
|```@ContextHierarchy```||
|```@ActiveProfiles```| Sets the active profiles for testing|
|```@TestPropertySource```| Used to configure property sources for test|
|```@DirtiesContext```|tells the testing framework to close and recreate the context for later tests|
|```@WebAppConfiguration```||
|```@TestExecutionListeners```| Allows to specify listeners for test events|
|```@Transactional``` | Used to mark a method as transactional|
|```@BeforeTranasaction```| code to execute b4 transactional|
|```@AfterTranasaction```| code to execute after transactional|
|```@Commit```| a test annotation that is used to indicate that a test-managed transaction should be committed after the test method has completed|
|```@Rollback```| |
|```@Sql```| declarative way to initialize and populate our test schema|
|```@SqlConfig```||
|```@SqlGroup```||
|```@Repeat```||
|```@Timed```||
|```@IfProfileValue```||
|```@ProfileValueSourceConfiguration```||

## References

* Spring Framework 5: Beginner to Guru Udemy course
* <https://www.baeldung.com/spring-boot-testing>
* <https://www.baeldung.com/spring-dirtiescontext>
