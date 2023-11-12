# Spring Data Rest

## Introduction
Spring Data REST is a framework that builds itself on top of the applications data repositories and expose those repositories in the form of REST endpoints. In order to make it easier for the clients to discover the HTTP access points exposed by the repositories, Spring Data REST uses hypermedia driven endpoints.
It reduces the need for a lot of boilerplate code in controller and service layer.
```java 
@RepositoryRestResource(collectionResourceRel = "sample", path = "sample")
public interface SampleRepository
extends CustomerRepository<sample, Long> {
```

It can be added by adding the following dependency in `pom.xml`
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-rest</artifactId>
</dependency>

```
## Syntax

```java
@RepositoryRestResource(collectionResourceRel = "users", path = "users")
public interface UserRepository extends PagingAndSortingRepository<WebsiteUser, Long> {
    List<WebsiteUser> findByName(@Param("name") String name);
}
```

All endpoints exposed can be found under `https://localhost:8080/` along with supported query params


## References
* https://www.codegrepper.com/profile/pragya-keshap
* https://spring.io/projects/spring-data-rest
* https://tanzu.vmware.com/developer/blog/getting-started-with-spring-data-rest-and-postgresql/
* https://www.baeldung.com/spring-data-rest-intro