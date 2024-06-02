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

## References
*  https://github.com/karatelabs/karate
* https://www.baeldung.com/karate-rest-api-testing
* https://github.com/karatelabs/karate/tree/master/karate-demo
* https://plugins.jetbrains.com/plugin/19232-karate