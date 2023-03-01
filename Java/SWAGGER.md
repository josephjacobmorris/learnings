# SWAGGER (OPENAPI)

## Introduction

Swagger is a tool used to generate REST API specification documentation.
Swagger specification can be written in YAML or JSON.

## Annontations

|Annonataion| Usage|
|:---------:|:------:|
|```@Operation(summary = "Get a book by its id")```|Add summary for Api|



```java
@ApiResponses(value = { 
  @ApiResponse(responseCode = "200", description = "Description for response", 
    content = { @Content(mediaType = "application/json") }),
  @ApiResponse(responseCode = "404", description = "Description for response", 
    content = @Content) })
```

## Miscellaneous

* <https://editor.swagger.io/> - helps in the case when api spec is being build as code is not written yet and it also gives an option to download a project template based on the apis

## References

* <https://www.baeldung.com/spring-rest-openapi-documentation>