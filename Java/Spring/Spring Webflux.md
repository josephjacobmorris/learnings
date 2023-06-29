# Spring Webflux

## Why reactive

In older spring mvc model for each request a thread was allocated from the thread pool of the server . 
By default, for tomcat it was 200. We can increase the number of threads and subsequently the number of requests that can be handled but only
upto a certain extent as thread are expensive resources which can easily take upto 1MB of the heap space. More memory occupied by the thread means
less memory to actually process the requests.

## Reactive Streams

Reactive stream specification :
* Publisher - It represents the source of data such as another m8s, database etc.
```java
public interface Publisher<T> {

    public void subscribe(Subscriber<? super T> s);
}
```
* Subscriber
```java
public interface Subscriber<T> {

    public void onSubscribe(Subscription s);

    public void onNext(T t);

    public void onError(Throwable t);

    public void onComplete();
}
```
* Subscription
```java
public interface Subscription {

    public void request(long n);

    public void cancel();
}
```
* Processor
```java
public interface Processor<T, R> extends Subscriber<T>, Publisher<R> {

}

```


The Contract of the above apis is part of flow api in jdk as of jdk 9.

## Reactor

## Non blocking api in spring
In spring webflux there are two ways to create non blocking apis,
* with annontated controllers
* with router functions (functional endpoints)

## Reference
* https://www.baeldung.com/reactor-core
