# MongoDB

## Introduction

Mongodb is a document store database. It follows BSON format ( a type of JSON format). It supports replicasets and shardings.

## Features

* Schema-less
* No complex joins required for querying
* Conversion/mapping of application objects to database objects not needed.

## Equivalent Terms in MongoDB by comparison with SQL

|RDBMS |MongoDB|
:-------:|:-------:|
|Database|Database|
|Table|Collection|
|Tuple/Row| Document|
|column|Field|
|Table join | Embedded Documents|

## Syntax

### Database

|Command |Usage|
:--------------:|:-------|
|```use <database-name>```|To switch to particular database. It creates the database if it already doesnt exist|
|```db.dropDatabase()```|To drop the current database|

### Collection

|Command |Usage|
:--------------:|:-------|
|```db.createCollection(“<name>”, options)```|create a collection|
|```db.COLLECTION_NAME.drop()```| drop a collection|

### Inserting data

|Command |Usage|
:--------------:|:--------------------------|
|```db.COLLECTION_NAME.insert(document)```|used to insert one or many documents (depreciated)|
|```db.COLLECTION_NAME.insertOne()```| used to insert one document|
|```db.COLLECTION_NAME.insertMany()```| used to insert multiple document|
|```db.COLLECTION_NAME.save()```| kind of upsert if _id field is present else behaves same as insert|

### Find (Select Equivalent in MongoDb )


|Command |Usage|
:--------------:|:--------------------------|
|```db.COLLECTION_NAME.find()```|Eq: ``` select * from COLLECTION; ``` |
|```db.COLLECTION_NAME.find().pretty()```| Using pretty() makes the output human readable and gives it proper formatting|

#### Examples

|Operator |Usage|
:--------------|:--------------------------|
|Equals|``` db.posts.find({“field1” : “value1”}) ``` |
|Less than|``` db.posts.find({“field1” : {$lt: 50}})``` |
|Greater than|``` db.posts.find({“field1” : {$gt: 50}})``` |
|Not Equals|``` db.posts.find({“field1” : {$ne: 50}})``` |
|In|``` db.posts.find({“field1” : {$in: {50, 60, 70}}})``` |
|Not in |``` db.posts.find({“field1” : {$nin: {50, 60, 70}}})``` |
|Less than or Equal to|``` db.posts.find({“field1” : {$lte: 50}})``` |
|Greater than or Equal to|``` db.posts.find({“field1” : {$gte: 50}})``` |
OR

``` 
db.mycol.find({
      $or: [{key1: value1}, {key2:value2}]
   }).pretty() 
```
AND, NOT
``` 
db.mycol.find({
      $and: [{key1: value1}, {key2:value2}]
   }).pretty() 
``` 
