# MongoDB

## Introduction

Mongodb is a document store database. It follows BSON format ( a type of JSON format). It supports replicasets and shardings.

### BSON
"BSON stands for “Binary JSON,” and that’s exactly what it was invented to be. BSON’s binary structure encodes type and length information, which allows it to be traversed much more quickly compared to JSON."

## Features

* Schema-less
* No complex joins required for querying
* Conversion/mapping of application objects to database objects not needed.
* High availability
* AP Database according to CAP theorm

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
|```show dbs```|To show all databases|
| ```db.getName()```|To get the name of the current database|

### Collection

|Command |Usage|
:--------------:|:-------|
|```db.createCollection(“<name>”, options)```|create a collection|
|```db.COLLECTION_NAME.drop()```| drop a collection|
| ```show collections```| show alls collections in the current db|
| ```db.getCollectionInfos()```| get the info about collection names and options in current db|

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

#### Projections

|Command |Usage|
:--------------:|:--------------------------|
|```db.COLLECTION_NAME.find({},{KEY:1})```| Eq : select key from collection.Note 1: what all to include |
|```db.COLLECTION_NAME.find({},{KEY:0})```| 0 denotes what fields to exclude|

Note:You cannot use both 0 and 1 in the same object. The only exception is the _id field. You should either specify the fields you would like to include or the fields you would like to exclude.

#### Limit,Skip and Sort
|Command |Usage|
:--------------:|:--------------------------|
|```db.COLLECTION_NAME.find().limit(NUMBER)```| Limits the number of documents displayed |
|```db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)```| Skips n number of documents |
|```db.COLLECTION_NAME.find().sort({KEY:1})```|Sorts o/p based on key and the order is 1: asc , -1 :desc|

### Update

|Command |Usage|
:--------------:|:--------------------------|
|```db.COLLECTION_NAME.update(SELECTION_CRITERIA, UPDATED_DATA)```| |
|```db.COLLECTION_NAME.findOneAndUpdate(SELECTIOIN_CRITERIA, UPDATED_DATA)```| update + fetch one|
|```db.COLLECTION_NAME.updateOne(<filter>, <update>)```| update one|
|```db.COLLECTION_NAME.updateMany(<filter>, <update>)```| |


### Delete

|Command |Usage|
:--------------:|:--------------------------|
|```db.COLLECTION_NAME.remove(DELETION_CRITTERIA)```| removes documents based on criteria|
|```db.mycol.remove({})```| Truncate equivalent|

## Indexing

Indexes support the efficient execution of queries in MongoDB. Without indexes, MongoDB must perform a collection scan, i.e. scan every document in a collection, to select those documents that match the query statement.

### Single Field Indexes

Indexes for whom the key is a single field. For a single-field index and sort operations, the sort order (i.e. ascending or descending) of the index key does not matter because MongoDB can traverse the index in either direction.

### Compound Index

MongoDb allows us to create indexes with mutliple fields. The order of index keys and sort order matters since in the index it will create the index in nested format.

When creating a compund index consider the following:

* #### ESR Rule (Equality, Sort & Range)

The index keys should in the format where fields which require Equality comparisons come first, followed by sort and then range so as to improve preformance.

* #### Covered Queries

A covered query is a query in which all the fields returned in the query are in the same index.Since all the fields present in the query are part of an index, MongoDB matches the query conditions and returns the result using the same index without actually looking inside the documents. Since indexes are present in RAM, fetching data from indexes is much faster as compared to fetching data by scanning documents.

* #### Ensure Indexes Fit in RAM

```db.collection.totalIndexSize()```

* #### Create Indexes to support your Queries

## Miscellaneous 

* mongoimport
* mongoexport
* mongodump