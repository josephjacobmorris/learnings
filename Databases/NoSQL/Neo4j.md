# Neo4j

## Introduction

Neo4j is a leading graph database that revolutionizes data management with its graph-oriented approach. Unlike
traditional relational databases, Neo4j stores data in nodes connected by relationships, enabling efficient data
traversal and complex relationship analysis. This allows for intuitive representation of highly connected data, making
it ideal for applications like social networks, recommendation engines, and fraud detection. Neo4j's flexible and
powerful query language, Cypher, facilitates easy retrieval of information from the graph. With its graph-centric
architecture, Neo4j offers unmatched performance and scalability, making it a go-to solution for businesses seeking to
harness the power of relationships in their data.

## Common Terms

### Labels

In Neo4j, labels are used to categorize and classify nodes in a graph database. A label is a descriptive tag or
identifier that can be assigned to nodes to group them based on their characteristics or properties. Labels help
organize and structure the data in the graph, making it easier to query and retrieve relevant information.

Key points about labels in Neo4j:

1. **Categorization of Nodes:** Labels are used to categorize nodes into different groups based on their common
   attributes or roles in the data model.

2. **Schema-Free:** Neo4j is a schema-free database, which means that nodes with different labels can have different
   properties, and there is no need to predefine a fixed schema.

3. **Indexed for Efficient Retrieval:** Neo4j uses indexes to speed up queries on labeled nodes, making it efficient to
   locate nodes with specific labels in large graphs.

4. **Multiple Labels per Node:** A node in Neo4j can have multiple labels, which allows it to belong to different
   categories simultaneously.

5. **Cypher Query Language:** When querying the graph, you can use labels as part of the Cypher query language to filter
   nodes based on their labels.

### Property

In Neo4j, properties are key-value pairs that are associated with nodes, relationships, and the database itself. They
represent the attributes or characteristics of nodes and relationships in the graph database. Properties allow you to
store and retrieve specific data for each node or relationship, making it possible to represent complex information
within the graph.

Key points about properties in Neo4j:

1. **Flexible Data Model:** Neo4j is a schema-free database, meaning that you can add properties to nodes and
   relationships dynamically without the need for a predefined schema.

2. **Property Types:** Properties in Neo4j can hold various data types, such as strings, numbers, booleans, dates, and
   arrays.

3. **Indexed for Efficient Queries:** You can create indexes on properties to speed up queries and make data retrieval
   more efficient.

4. **Cypher Query Language:** When querying the graph, you can use properties as part of the Cypher query language to
   filter nodes and relationships based on their property values.

5. **Unique Constraint:** You can create a unique constraint on a property to ensure that no two nodes or relationships
   have the same value for that property.

## Syntax

### Database

|          Command          | Description                              |
|:-------------------------:|:-----------------------------------------|
| `create database student` | creates database                         |
|  `use database student`   | switches the current database to student |
|  `drop database student`  | drops the student database               |

### Node

|                                                     Command                                                      | Description                                                                          |
|:----------------------------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------|
|                                                   `create ()`                                                    | create node without label                                                            |
|                                               `create (:Student)`                                                | create node with label Student                                                       |
|                                            `create (:Student:Person)`                                            | create node with label Student, Person                                               |
|                                      `create (st:Student:Person) return st`                                      | create node with label Student, Person and returns the node                          |
|                                  `create (st:Student){name : 'John'} return st`                                  | create node with label Student , property name and returns the node                  |
|                                         `match(st:Student{name:'John'})`                                         | returns all nodes with label student and name property = 'John`                      |
|                         `match(st:Student) where st.name='John' and st.birth_year=1990`                          | returns all nodes with label student and name property = 'John` and birth_year =1990 |
|                        `match(st:Student) where st.birth_year=1990 OR st.birth_year=1991`                        | returns all nodes with label student  and birth_year =1990  or 1991                  |
|                              `match(st:Student) where st.birth_year IN [1990,1991]`                              | returns all nodes with label student  and birth_year =1990  or 1991                  |
|                                       `match(st:Student) where ID(st) = 4`                                       | returns all nodes with label student  and ID = 4                                     |
| `MATCH (n:OldLabel) WHERE ID(n) = 4 SET n:NewLabel SET n.property1 = 'new value 1', n.property2 = 'new value 2'` | sample query to add new label and upsert properties                                  |
|                        `MATCH (n:Student) WHERE ID(n) = 4 remove n:Student , n.property1`                        | sample query to remove  label and  properties                                        |
|                                   `MATCH (n:Student) WHERE ID(n) = 4 delete n`                                   | sample query to delete node without relationships                                    |
|                                  `MATCH (n) WHERE SIZE(labels(n)) = 0 delete n`                                  | sample query to delete node without relationships and labels                         |

### Relationship

|                                                                                           Command                                                                                           | Description                                                            |
|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-----------------------------------------------------------------------|
|                                          `CREATE (node1:Label1 {property1: 'value1'})-[:RELATIONSHIP_TYPE]->(node2:Label2 {property2: 'value2'})`                                           | create relationship between two nodes at the time of creation of nodes |
|                       <pre> MATCH (node1:Label1 {property1: 'value1'}), (node2:Label2 {property2: 'value2'}) <br> CREATE (node1)-[:RELATIONSHIP_TYPE]->(node2)</pre>                        | create relationship between two existing nodes                         |
|                                        <pre> MATCH (node1:Label1)-[rel]-(node2:Label2)<br>RETURN node1, TYPE(rel) AS relationship_type, node2 </pre>                                        | find relationship between two existing nodes                           |
| <pre> MATCH (startNode:Label1)-[r:RELATIONSHIP_TYPE]->(endNode:Label2)<br>WHERE r.property_name = 'old_value'<br> SET r.property_name = 'new_value'<br> RETURN startNode, r, endNode </pre> | update relationship property between two existing nodes                |
|      <pre> MATCH (startNode:Label1)-[r:RELATIONSHIP_TYPE]->(endNode:Label2)<br>WHERE r.property_name = 'old_value'<br> remove r.property_name <br> RETURN startNode, r, endNode </pre>      | remove relationship property between two existing nodes                |
|             <pre> MATCH (startNode:Label1)-[r:RELATIONSHIP_TYPE]->(endNode:Label2)<br>WHERE r.property_name = 'old_value'<br> delete r <br> RETURN startNode, r, endNode </pre>             | delete relationship  between two existing nodes                        |
|   <pre>MATCH (startNode)-[oldRel:OLD_TYPE]->(endNode)<br>MERGE (startNode)-[newRel:NEW_TYPE]->(endNode)<br>SET newRel = oldRel <br>DELETE oldRel<br> RETURN startNode, r, endNode </pre>    | update relationship type  between two existing nodes                   |
|                                                             <pre> MATCH (n)-[r]-(m) WHERE ID(n) = 4 DETACH DELETE n, r, m</pre>                                                             | delete a node with relationships                                       |
|                                                                          <pre> MATCH (n)<br>DETACH DELETE n</pre>                                                                           | delete all node even those with relationships                          |
|                                                              <pre> MATCH (n)<br>OPTIONAL MATCH (n)-[r]-()<br>DELETE n, r</pre>                                                              | delete all node even those with relationships                          |

### Constraint

|                                         Command                                          | Description                                                                       |
|:----------------------------------------------------------------------------------------:|:----------------------------------------------------------------------------------|
|  <pre> CREATE CONSTRAINT cn1 ON (node:Label1) ASSERT node.property_name IS UNIQUE</pre>  | create unique constraint on node property. It will allow null values              |
| <pre> CREATE CONSTRAINT cn2 ON (node:Label1) ASSERT node.property_name IS NODE KEY</pre> | create unique and not null constraint on node property                            |
|  <pre> CREATE CONSTRAINT cn3  ON (node:Label1) ASSERT exists(node.property_name) </pre>  | create not null constraint on node property. It will work with relationships also |
|                               <pre>SHOW CONSTRAINTS </pre>                               | show all constraints in database                                                  |
|                       <pre>DROP CONSTRAINT constraint_name </pre>                        | drop constraint with given name                                                   |

### String Comparisons

|                                 Command                                  | Description |
|:------------------------------------------------------------------------:|:------------|
|  <pre> match(st:Student) where st.name ends with 'son' return st </pre>  |             |
| <pre> match(st:Student) where st.name starts with 'son' return st </pre> |             |
|  <pre> match(st:Student) where st.name contains 'son' return st </pre>   |             |

### Aggregate Functions

| Neo4j Aggregate Function Example                                                                       | Description                                                                                                            |
|--------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| `MATCH (n:Label) RETURN COUNT(n) AS count`                                                             | Returns the total count of nodes with a specific label (`Label`).                                                      |
| `MATCH (n:Label) RETURN SUM(n.property) AS total`                                                      | Calculates the sum of the specified property (`property`) for all nodes with a specific label (`Label`).               |
| `MATCH (n:Label) RETURN AVG(n.property) AS average`                                                    | Calculates the average of the specified property (`property`) for all nodes with a specific label (`Label`).           |
| `MATCH (n:Label) RETURN MIN(n.property) AS min`                                                        | Finds the minimum value of the specified property (`property`) among all nodes with a specific label (`Label`).        |
| `MATCH (n:Label) RETURN MAX(n.property) AS max`                                                        | Finds the maximum value of the specified property (`property`) among all nodes with a specific label (`Label`).        |
| `MATCH (n:Label) RETURN COLLECT(n.property) AS values`                                                 | Collects all the values of the specified property (`property`) into a list for nodes with a specific label (`Label`).  |
| `MATCH (n:Label) RETURN COLLECT(DISTINCT n.property) AS unique_values`                                 | Collects distinct values of the specified property (`property`) into a list for nodes with a specific label (`Label`). |
| `MATCH (n:Label) WITH COLLECT(n.property) AS values RETURN STDEV(values) AS stdev`                     | Calculates the standard deviation of the specified property (`property`) for nodes with a specific label (`Label`).    |
| `MATCH (n:Label) WITH COLLECT(n.property) AS values RETURN percentileDisc(values, 0.75) AS percentile` | Calculates the 75th percentile of the specified property (`property`) for nodes with a specific label (`Label`).       |
| `MATCH (n:Label) WITH COLLECT(n.property) AS values RETURN percentileCont(values, 0.95) AS percentile` | Calculates the 95th percentile of the specified property (`property`) for nodes with a specific label (`Label`).       |

### Pagination and sorting

| Neo4j Pagination & Sorting Example                 | Description                                                                                                                      |
|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| `MATCH (n:Label) RETURN n SKIP 10 LIMIT 5`         | Performs pagination by skipping the first 10 results and returning the next 5 results for nodes with a specific label (`Label`). |
| `MATCH (n:Label) RETURN n ORDER BY n.property ASC` | Sorts the results in ascending order based on the specified property (`property`) for nodes with a specific label (`Label`).     |

## Spring & Neo4j
`build.gradle`
```groovy
// Spring Boot WebFlux and Spring Data Neo4j dependencies
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-webflux'
    implementation 'org.springframework.boot:spring-boot-starter-data-neo4j'
}
```
```java
@Node
public class Person {
    @Id @GeneratedValue
    private Long id;
    private String name;
    private int age;

    @Relationship(type = "FRIEND_OF")
    private List<Friendship> friendships = new ArrayList<>();

    // Getters, setters, and other properties.
}

@Node
public class Hobby {
    @Id @GeneratedValue
    private Long id;
    private String name;
    private String description;

    // Getters, setters, and other properties.
}

@RelationshipProperties
public class Friendship {
    @Id @GeneratedValue
    private Long id;
    private LocalDate since;

    // Getters, setters, and other properties.
}

```

`application.yaml`
```yaml
spring:
  data:
    neo4j:
      uri: bolt://localhost:7687
      username: neo4j
      password: your_password

```

### See generated Queries
> logging.level.org.springframework.data.neo4j=DEBUG

### Sorting & Pagination
1. Sorting:

To sort the results based on a specific property, you can use the `Sort` class. The `Sort` class allows you to specify one or more properties and their sorting direction (ascending or descending).

```java
import org.springframework.data.domain.Sort;

// ...

Sort sortByNameAsc = Sort.by(Sort.Direction.ASC, "name");
Iterable<Person> sortedPersons = personRepository.findAll(sortByNameAsc);
```

2. Pagination:

To perform pagination, you can use the `PageRequest` class. The `PageRequest` class allows you to specify the page number, the number of items per page (page size), and the sorting criteria.

```java
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Sort;

// ...

Sort sortByAgeDesc = Sort.by(Sort.Direction.DESC, "age");
PageRequest pageRequest = PageRequest.of(1, 10, sortByAgeDesc); // Page number starts from 0
Page<Person> pageOfPersons = personRepository.findAll(pageRequest);
List<Person> personsOnPage = pageOfPersons.getContent();
```

### Starts with, Ends With & Contains
 Spring Data Neo4j provides method proxying and supports various keyword-based query methods that allow you to perform string matching operations such as `STARTS WITH`, `ENDS WITH`, `CONTAINS`, and more. These methods leverage Spring Data's query generation capabilities, making it more convenient to perform string matching without the need to write custom Cypher queries explicitly.

Here's how you can use some of these method proxying options in Spring Data Neo4j:

1. **STARTS WITH:**
   To perform the `STARTS WITH` operation, you can use the `findBy<Property>StartsWith` method naming convention in your repository interface. For example:

```java
public interface PersonRepository extends Neo4jRepository<Person, Long> {
    List<Person> findByNameStartsWith(String prefix);
}
```

2. **ENDS WITH:**
   For the `ENDS WITH` operation, you can use the `findBy<Property>EndsWith` method naming convention. For example:

```java
public interface PersonRepository extends Neo4jRepository<Person, Long> {
    List<Person> findByNameEndsWith(String suffix);
}
```

3. **CONTAINS:**
   For the `CONTAINS` operation, you can use the `findBy<Property>Containing` or `findBy<Property>Like` method naming conventions. Both options will perform a substring search. For example:

```java
public interface PersonRepository extends Neo4jRepository<Person, Long> {
    List<Person> findByNameContaining(String substring);
    // Or
    List<Person> findByNameLike(String substring);
}
```
## References

* Chatgpt