# Karate

## Introduction

## Syntax

### Background
The Background keyword is used to define a set of steps that are common to all the scenarios in a feature file. These steps are executed before each scenario in the feature file, allowing you to set up a common context or state that is required for multiple scenarios. This helps to avoid repetition and makes the feature files cleaner and more maintainable.
```
Feature: User Login

  Background:
    Given url 'https://example.com/api'
    And path 'login'
    And request { username: 'testuser', password: 'testpass' }
  
  Scenario: Successful login
    When method post
    Then status 200
    And match response.message == 'Login successful'

  Scenario: Unsuccessful login due to wrong password
    Given request { username: 'testuser', password: 'wrongpass' }
    When method post
    Then status 401
    And match response.error == 'Invalid credentials'
```

### URL
The url keyword is used to define the base URL for the HTTP requests that will be made during the execution of the test scripts. This is particularly useful for setting up the environment and ensuring that all the HTTP requests are directed to the correct server or endpoint.

### PATH
the path keyword is used to specify the endpoint path for an HTTP request. It helps to construct the URL dynamically by appending the provided path to the base URL defined in the Karate configuration. This keyword is particularly useful when you need to test multiple endpoints under the same base URL.

### Query Parameters
```
Given param limit = 10
Given param offset = 0
```
or 
```
Given params { limit: 10, offset:0 }
```

### Assertions

Simple assertions are used to validate the responses of API calls. Here are some of the different simple assertions possible in Karate:

### 1. **Equality Assertion**
To check if a particular value is equal to an expected value.

```karate
* def response = { id: 123, name: 'John Doe', age: 30 }
* match response.id == 123
* match response.name == 'John Doe'
* match response.age == 30
```

### 2. **Containment Assertion**
To check if a collection contains a specific element or if a string contains a substring.

```karate
* def response = { fruits: ['apple', 'banana', 'cherry'] }
* match response.fruits contains 'banana'
* match response.fruits contains ['banana', 'cherry']
```

### 3. **Type Assertion**
To check if a value is of a specific type.

```karate
* def response = { id: 123, name: 'John Doe', age: 30 }
* match response.id == '#number'
* match response.name == '#string'
* match response.age == '#number'
```

### 4. **Schema Assertion**
To check if an object adheres to a specified schema.

```karate
* def response = { id: 123, name: 'John Doe', age: 30 }
* match response == { id: '#number', name: '#string', age: '#number' }
```

### 5. **Contains Only Assertion**
To check if a collection contains exactly the specified elements, regardless of the order.

```karate
* def response = { fruits: ['apple', 'banana', 'cherry'] }
* match response.fruits contains only ['banana', 'apple', 'cherry']
```

### 6. **Contains Any Assertion**
To check if a collection contains any one of the specified elements.

```karate
* def response = { fruits: ['apple', 'banana', 'cherry'] }
* match response.fruits contains any ['banana', 'mango']
```

### 7. **Not Contain Assertion**
To check if a collection does not contain a specific element.

```karate
* def response = { fruits: ['apple', 'banana', 'cherry'] }
* match response.fruits !contains 'mango'
```

### 8. **Size Assertion**
To check the size of a collection.

```karate
* def response = { fruits: ['apple', 'banana', 'cherry'] }
* match response.fruits.length == 3
```

### 9. **Null Assertion**
To check if a value is null or not null.

```karate
* def response = { id: 123, name: null, age: 30 }
* match response.name == null
* match response.age != null
```

### 10. **Regex Assertion**
To check if a string matches a regular expression.

```karate
* def response = { email: 'john.doe@example.com' }
* match response.email == /^.+@.+\..+$/
```

### 11. **Range Assertion**
To check if a number falls within a specific range.

```karate
* def response = { score: 85 }
* match response.score > 80
* match response.score < 90
```

These assertions provide powerful and flexible ways to validate API responses in Karate, ensuring that your API behaves as expected.

## References
* Chatgpt
*  https://github.com/karatelabs/karate
* https://www.baeldung.com/karate-rest-api-testing
* https://github.com/karatelabs/karate/tree/master/karate-demo
* https://plugins.jetbrains.com/plugin/19232-karate