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

## References

* Chatgpt