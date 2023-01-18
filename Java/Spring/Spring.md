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
