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

### Global ErrorHandler
```java
@Component
@Slf4j
public class GlobalErrorHandler implements ErrorWebExceptionHandler {
    @Override
    public Mono<Void> handle(ServerWebExchange exchange, Throwable ex) {
        log.error("Exception message is {} ", ex.getMessage(), ex);
        DataBufferFactory dataBufferFactory = exchange.getResponse().bufferFactory();
        var errorMessage = dataBufferFactory.wrap(ex.getMessage().getBytes());

        if(ex instanceof ReviewDataException){
            exchange.getResponse().setStatusCode(HttpStatus.BAD_REQUEST);
            return  exchange.getResponse().writeWith(Mono.just(errorMessage));
        }
        if(ex instanceof ReviewNotFoundException){
            exchange.getResponse().setStatusCode(HttpStatus.NOT_FOUND);
            return  exchange.getResponse().writeWith(Mono.just(errorMessage));
        }
        exchange.getResponse().setStatusCode(HttpStatus.INTERNAL_SERVER_ERROR);
        return  exchange.getResponse().writeWith(Mono.just(errorMessage));
    }
}
```

### Retry in Webclient
Spring WebFlux's WebClient provides several retry mechanisms to handle transient failures when making HTTP requests. Let's explore three common retry mechanisms: fixed delay, exponential backoff, and retry with custom retry conditions.

#### Fixed Delay Retry:
This mechanism retries the request after a fixed delay if the previous attempt fails. Here's an example:
```java
import org.springframework.http.HttpStatus;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.retry.Retry;
import reactor.retry.RetryBackoffSpec;

WebClient webClient = WebClient.builder().baseUrl("https://api.example.com").build();

WebClient.RequestHeadersUriSpec<?> request = webClient.get().uri("/endpoint");

Retry retry = Retry.fixedDelay(3, Duration.ofSeconds(2))
                .filter(throwable -> throwable instanceof IOException);

String response = request.retrieve()
                         .retryWhen(retry)
                         .bodyToMono(String.class)
                         .block();
```

#### Exponential Backoff Retry:
This mechanism increases the delay between retries exponentially. Here's an example:
```java
import org.springframework.http.HttpStatus;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.retry.Retry;
import reactor.retry.RetryBackoffSpec;

WebClient webClient = WebClient.builder().baseUrl("https://api.example.com").build();

WebClient.RequestHeadersUriSpec<?> request = webClient.get().uri("/endpoint");

RetryBackoffSpec backoffSpec = Retry.backoff(3, Duration.ofSeconds(2))
                                   .maxBackoff(Duration.ofSeconds(10));

String response = request.retrieve()
                         .retryWhen(backoffSpec)
                         .bodyToMono(String.class)
                         .block();
```
#### Retry with Custom Retry Conditions:
This mechanism allows you to define custom retry conditions based on the response or throwable. Here's an example:
```java
import org.springframework.http.HttpStatus;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.retry.Retry;
import reactor.retry.RetryPredicate;

WebClient webClient = WebClient.builder().baseUrl("https://api.example.com").build();

WebClient.RequestHeadersUriSpec<?> request = webClient.get().uri("/endpoint");

Retry retry = Retry.anyOf(IOException.class, TimeoutException.class)
                .retryMax(3)
                .doOnRetry(context -> System.out.println("Retrying..."));

String response = request.retrieve()
                         .retryWhen(retry)
                         .bodyToMono(String.class)
                         .block();


```
#### Retry with Circuit Breaker:
This mechanism combines retries with circuit-breaking functionality. It stops retrying if a certain number of consecutive failures occur. Here's an example:
```java
import org.springframework.http.HttpStatus;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.retry.Retry;
import reactor.retry.RetryContext;
import reactor.retry.RetryPredicate;

WebClient webClient = WebClient.builder().baseUrl("https://api.example.com").build();

WebClient.RequestHeadersUriSpec<?> request = webClient.get().uri("/endpoint");

Retry retry = Retry.anyOf(CustomRetryException.class)
                .retryMax(3)
                .withCircuitBreaker(2, Duration.ofSeconds(10))
                .doOnRetry(context -> System.out.println("Retrying..."));

String response = request.retrieve()
                         .retryWhen(retry)
                         .bodyToMono(String.class)
                         .block();

```

## WireMock
Wiremock is used to mock interaction between mutliple m8s via HTTP request in testcases

```java
import com.github.tomakehurst.wiremock.client.WireMock;
import com.github.tomakehurst.wiremock.core.WireMockConfiguration;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.web.reactive.function.client.WebClient;

import static com.github.tomakehurst.wiremock.client.WireMock.*;

@ExtendWith(SpringExtension.class)
@WireMockTest(httpPort = 8080)
public class WireMockAnnotationExample {

    @Test
    public void exampleTest() {
        // Configure WireMock to stub a GET request
        stubFor(get(urlEqualTo("/api/example"))
                .willReturn(aResponse()
                        .withStatus(HttpStatus.OK.value())
                        .withHeader("Content-Type", MediaType.APPLICATION_JSON_VALUE)
                        .withBody("{\"message\": \"Hello, WireMock!\"}")));

        // Make an HTTP request to the stubbed endpoint
        String response = WebClient.create("http://localhost:8080")
                .get()
                .uri("/api/example")
                .retrieve()
                .bodyToMono(String.class)
                .block();

        // Print the response
        System.out.println("Response: " + response);
    }
}

```

## Reference
* https://www.baeldung.com/reactor-core
* https://www.baeldung.com/netty
