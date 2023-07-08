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

## Testing in Spring webflux

### Approach 1
```java
    @Test
    void approach1() {

        webTestClient.
                get()
                .uri("/endpoint")
                .exchange()
                .expectStatus()
                .is2xxSuccessful()
                .expectBodyList(String.class)
                .hasSize(3);
    }
```

### Approach 2
```java
    @Test
    void approach2() {

        var fluxOp = webTestClient.
                get()
                .uri("/endpoint")
                .exchange()
                .expectStatus()
                .is2xxSuccessful()
                .returnResult(String.class)
                .getResponseBody();

        StepVerifier.create(fluxOp)
                .expectNext("1","2","3")
                .verifyComplete();
    }

```
### Approach 3
```java
    @Test
    void approach3() {
        webTestClient.
                get()
                .uri("/endpoint")
                .exchange()
                .expectStatus()
                .is2xxSuccessful()
                .expectBodyList(String.class)
                .consumeWith(list -> {
                    var responseBody = list.getResponseBody();
                    assert (Objects.requireNonNull(responseBody).size() == 3);
                });
    }
```
## How Netty works in a non-blocking fashion ?

### Channels

### EventLoops

### EventLoopGroups

## Functions Web (Router-Handler) Approach
In this style of writing rest endpoints there will be two components :

* Router - which will ahve the equivalent to the `@xxxMapping` annotations
* Handler - which will have the equivalent of the method body for `@xxxMapping` annotations

### Nest Function

```java
ublic RouterFunction<ServerResponse> bookRoutes(BookHandler handler) {
        return nest(path("/books"),
                route(GET("/"), handler::getAllBooks)
                        .andRoute(POST("/"), handler::createBook)
                        .nest(path("/{id}"),
                                route(GET("/"), handler::getBookById)
                                        .andRoute(PUT("/"), handler::updateBook)
                                        .andRoute(DELETE("/"), handler::deleteBook)
                        )
        );
    }
```

### Validatation
* Add the necessary validation annontations in the DTO
* autowire the validator bean 
```java
@Autowired
// add the validation 
private Validator validator;
```
* validate using the `validator` like below (object is the object to be validate)
```java
var errors = validator.validate(object);
        if (errors.isEmpty()) {
            return object;
        } else {
            String errorDetails = errors.stream().map(er -> er.getMessage()).collect(Collectors.joining(", "));
            throw new ObjectValidationException(errorDetails);
        }
```

## Reference
* https://www.baeldung.com/reactor-core
* https://www.baeldung.com/netty
