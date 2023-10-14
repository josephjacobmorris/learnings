**Introduction to Spring Modulith**

In this article, the author introduces Spring Modulith, an experimental Spring project aimed at helping developers structure well-organized, domain-aligned Spring Boot applications. It addresses the need for applications to evolve and adapt to changing business requirements while focusing on code structure alignment with the domain.

**The Need for Evolvable Software**

Architects and developers have various architectural options to choose from when designing software systems, including microservices and modular monolithic systems. Regardless of the chosen architectural style, applications need to have a structure that can adapt to changes in business requirements.

**Traditional Guidance and the Shift Towards Domain-Aligned Structure**

Traditionally, application frameworks provided structural guidance using technical abstractions like Spring Framework's stereotype annotations. However, aligning code structure with the domain has proven to result in better-structured, understandable, and maintainable applications.

**Introducing Spring Modulith**

Spring Modulith is a new, experimental Spring project that helps developers express logical application modules in code. It aids in building well-structured, domain-aligned Spring Boot applications.

**Example Project Structure**

The article provides an example project structure for an e-commerce application consisting of two logical modules: order processing and inventory management. These modules are organized into packages that distinguish between public and private types.

- The "order" module contains sub-packages with a public Spring bean.
- The "inventory" module consists of a single package with non-public types.

**Verifying the Modular Structure**

To ensure the application structure adheres to defined conventions, developers can create a test case that checks the modular structure using Spring Modulith's capabilities. This includes validating that dependencies between modules are correctly established.

**Application Module Integration Tests**

Spring Modulith allows developers to isolate integration tests for specific application modules. Tests can focus on specific modules and their dependencies, making it easier to pinpoint issues and changes.

**Using Events for Inter-module Interaction**

The article emphasizes using application events for inter-module communication instead of direct dependencies between modules. Spring Modulith provides a way to test whether specific events are published during integration tests.

**A Toolbox for Well-structured Spring Boot Applications**

Spring Modulith offers many features to help structure Spring Boot applications, such as advanced package arrangements, flexible selection of modules for integration tests, transaction event publication logs, developer documentation generation, runtime observability, and more.

**About Moduliths**

Spring Modulith is the continuation of the Moduliths project, utilizing newer technologies and frameworks. The older Moduliths project is compatible with Spring Boot 2.7 and will be maintained accordingly.


## References
* Chatgpt
* https://spring.io/blog/2022/10/21/introducing-spring-modulith