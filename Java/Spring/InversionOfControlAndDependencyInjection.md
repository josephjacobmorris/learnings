# Inversion Of Control and Dependency Injection

## Inversion of Control (IoC)

It is the priciple in software engineering by which control of some portion or piece of code is given to the underlying f/ws.
In Spring Framework ApplicationContext is the IoC.

### Adavantages of Using IoC

* Reduces coupling
* Greater Ease in testing since one component can be mocked.

## Dependency Injection

"It is pattern we can use to implement IoC, where the control being inverted is setting an object's dependencies". Dependency Injection can be done by injecting a dependency through the constructor, setter and Field based injection

Say we have a service greeting service and its implementation like this:
```java
package com.example.pricipleexample.services;

public interface GreetingService {
    String sayGreeting();
}
```

```java
package com.example.pricipleexample.services;

public class GreetingServiceImpl implements GreetingService {
    @Override
    public String sayGreeting() {
        return "Hi Folks!";
    }
}
```

Then the dependency injection can be done via:

### Property Based Injection

```java
package com.example.pricipleexample.controllers;

import com.example.pricipleexample.services.GreetingService;

public class PropertyInjectedController {
    public GreetingService greetingService;

    public String getGreeting() {
        return greetingService.sayGreeting();
    }
}

```

### Setter Based Injection

```java
package com.example.pricipleexample.controllers;

import com.example.pricipleexample.services.GreetingService;

public class SetterInjectedController {
    private  GreetingService greetingService;

    public void setGreetingService(GreetingService greetingService) {
        this.greetingService = greetingService;
    }

    public String getGreeting(){
        return greetingService.sayGreeting();
    }
}
```

### Constructor Based Injection

```java
package com.example.pricipleexample.controllers;

import com.example.pricipleexample.services.GreetingService;

public class ConstructorInjectedController {
    private final GreetingService greetingService;

    public ConstructorInjectedController(GreetingService greetingService) {
        this.greetingService = greetingService;
    }

    public String getGreeting() {
        return greetingService.sayGreeting();
    }
}

```

## Inversion of Control in Spring

### Property based Dependency Injection using Spring

```java
@Controller
public class PropertyInjectedController {
    @Autowired
    public GreetingService greetingService;

    public String getGreeting() {
        return greetingService.sayGreeting();
    }
}
```

```java
@Component
public class GreetingServiceImpl implements GreetingService {
    @Override
    public String sayGreeting() {
        return "Hi Folks!";
    }
}
```

```@Autowired``` annonation on property tells spring to inject the qualifying bean for the property. Property based Dependency injection is not recommened especially if its a private property since spring would have to use java reflection to inject the dependency for the property which will introduce a performance overhead.

### Setter based Dependency Injection using Spring

```java
@Controller
public class PropertyInjectedController {

    private GreetingService greetingService;

    @Autowired
    public void setGreetingService(GreetingService greetingService) {
        this.greetingService = greetingService;
    }

    public String getGreeting() {
        return greetingService.sayGreeting();
    }
}
```

The difference with property based injection is that ```@Autowired``` annotation is given to the setter method.

### Constructor based Dependency Injection using Spring

```java
@Controller
public class PropertyInjectedController {

    private final GreetingService greetingService;
    
    public PropertyInjectedController(GreetingService greetingService) {
        this.greetingService = greetingService;
    }

    public String getGreeting() {
        return greetingService.sayGreeting();
    }
}
```
Note: How the ```@Autowired``` annonation is missing for constructed based injection.

<em> But what if multiple components qualify for the same injection?</em>

Then based on the 

## Reference

* <https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring>
