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
This can be handled  ```@Qualifier``` , ```@Primary``` and ```@Profile```

```@Profile``` Indicates that a component is eligible for registration when one or more specified profiles are active.
A profile is a named logical grouping that may be activated programmatically via ConfigurableEnvironment.setActiveProfiles or declaratively by setting the spring.profiles.active property as a JVM system property, as an environment variable, or as a Servlet context parameter in web.xml for web applications. Profiles may also be activated declaratively in integration tests via the @ActiveProfiles annotation.

```@Primary``` - Indicates that a bean should be given preference when multiple candidates are qualified to autowire a single-valued dependency. If exactly one 'primary' bean exists among the candidates, it will be the autowired value.

```@Qualifier``` - This annotation may be used on a field or parameter as a qualifier for candidate beans when autowiring. It may also be used to annotate other custom annotations that can then in turn be used as qualifiers.

```java
@Controller
public class PropertyInjectedController {

    private final GreetingService greetingService;

    public PropertyInjectedController(@Qualifier("greetingServiceImpl")GreetingService greetingService) {
        this.greetingService = greetingService;
    }

    public String getGreeting() {
        return greetingService.sayGreeting();
    }
}
```

The order of precendence is ```@Profile``` > ```@Qualifier``` > ```@Primary```.

## Reference

* <https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring>
* Spring Java Docs
