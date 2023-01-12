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

