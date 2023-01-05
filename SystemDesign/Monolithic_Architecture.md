# Monolithic Architecture

## Introduction

## Pros

* Simple to develop Initally.
* Easy to debug
* Easy to deploy since  there will be only one package (war, jar,... etc)
* Communication between modules is easy

## Cons

* Gets complicated to understand in the long run.
* In the future , the chances of bugs in new features will increase exponentially.
* Introduction of new technologies is very difficult. 
* Scalability of individual modules is not possible.

## Communication of Components

Since in monolithic architecture all components are in the same server , it is very easy to communicate betweeen them.
It is done by inter-process communication, which is provided by the OS , using method calls.

## Transaction Management

It is easy to do transaction management since only one database (mostly RDBMS) is used. we can use the transaction feature from the database itself in most of the scenarios.