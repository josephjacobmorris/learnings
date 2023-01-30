# Hibernate

## Introduction

"Hibernate is a high-performance Object/Relational persistence and query service.". Hibernate is an ORM (Object Relational Mapping) f/w Basically, it means that when we modify the objects we can persist it into the database without writing sql queries.

## Advantages

* Hides SQL queries from buisness logic.
* Makes application development easier and faster.
* Transaction management and automatic key generation.

## Supported Databases

* HSQL Database Engine
* DB2/NT
* MySQL
* PostgreSQL
* FrontBase
* Oracle
* Microsoft SQL Server Database
* Sybase SQL Server
* Informix Dynamic Server

## Sample Entity

```java
import jakarta.persistence.*;

import java.util.HashSet;
import java.util.Objects;
import java.util.Set;

@Entity
public class Author {
    private String firstName;

    private String lastName;

    @ManyToMany(mappedBy = "authors")
    private Set<Book> books;

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    public Author() {
    }

    public Author(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.books = new HashSet<>();
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public Set<Book> getBooks() {
        return books;
    }

    public void setBooks(Set<Book> books) {
        this.books = books;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public Long getId() {
        return id;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Author author = (Author) o;

        return Objects.equals(id, author.id);
    }

    @Override
    public int hashCode() {
        return id != null ? id.hashCode() : 0;
    }

    @Override
    public String toString() {
        return "Author{" +
                "firstName='" + firstName + '\'' +
                ", lastName='" + lastName + '\'' +
                ", books=" + books +
                ", id=" + id +
                '}';
    }
}
```

### Entity Annotation

The  ```@Entity``` annotation specifies that the class is correlated with a table in the database.

* The class must have a public or protected, no-argument constructor. The class may have other constructors.

* The class must not be declared final. No methods or persistent instance variables must be declared final.

* If an entity instance is passed by value as a detached object, such as through a session bean’s remote business interface, the class must implement the Serializable interface.

* Entities may extend both entity and non-entity classes, and non-entity classes may extend entity classes.

* Persistent instance variables must be declared private, protected, or package-private and can be accessed directly only by the entity class’s methods. Clients must access the entity’s state through accessor or business methods.

### Id Annotation

The  ```@Id``` annotation specifies the primary key of an entity. The field or property to which the Id annotation is applied should be one of the following types: any Java primitive type; any primitive wrapper type; String; java.util.Date; java.sql.Date; java.math.BigDecimal; java.math.BigInteger.
The mapped column for the primary key of the entity is assumed to be the primary key of the primary table. If no Column annotation is specified, the primary key column name is assumed to be the name of the primary key property or field.

### GeneratedValue Annotation

Provides for the specification of generation strategies for the values of primary keys.
The GeneratedValue annotation may be applied to a primary key property or field of an entity or mapped superclass in conjunction with the Id annotation. The use of the GeneratedValue annotation is only required to be supported for simple primary keys.

## Types of Relationship

* ### One-to-one relationship

* ### One-to-Many relationship

* ### Many-to-one relationship

The inverse relationship of one-to-many relationship and makes the relationship bi-directional.

* ### Many-to-many relationship


## Birectional Relationship vs Unidirectional Relationship

### Birectional Relationship

both sides of the relationship know each other.

### Unidirectional Relationship

only one side of the relationship knows the other side.

## Owning Side
The side of the relationship which holds the foreign key in the database.

* One to one is the side where foreign key is specified

* One to many and many to one is the many side.

## Fetch Type

* Lazy 

* Eager

## JPA Cascade Type

### Persist

### Merge

### Refresh

### Remove

### DETACH

### ALL

All of the above gets applied.

### By default none is applied


## Scratch Pad

Can use <https://start.jhipster.tech/jdl-studio/> for data modeling

## References

* <https://stackoverflow.com/questions/63414381/what-is-entity-in-spring-jpa>
* <https://www.tutorialspoint.com/hibernate/hibernate_overview.htm>
* Java Documentation