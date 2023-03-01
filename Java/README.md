# JAVA

**Version :** Java 17

## Steps to install

### In Ubuntu

 ```bash
sudo apt update
sudo apt install -y openjdk-17-jdk
java -version #confirm installation
```

## To see compiler instructions corresponding to code

* <https://godbolt.org/>

## JShell

It became part of JDK as of java 9. It is what is known as REPL (Read Evalaute Print Loop) interactive program.
Basically

* it reads command or code snippet from us
* it evaluates and executes command/code and allows shortcuts
* it prints the result of evaluation/execution
* it again waits for command/code snippet

## Keywords
 
 List of keywords : <https://docs.oracle.com/javase/specs/jls/se17/html/jls-3.html#jls-3.9>

 ## SOLID Principles

### Single Responsiblity Principle

 Single Responsiblity Principle states that a class should have only one resposibility. It should have only one reason for change. Avoid coding 'God classes'. It reduces the number of bugs and is easy to understand by others. It reduces the time required to implement features.

### Open-Closed Principle

It states that a class should be closed for modification and open to extensions. It was originally proposed by Bertrand Mayer. But as discussed by Robert.C. Martin and others a better approach is to use interfaces instead of superclasses which allow for different implementations. This approach is called the polymorphic open-closed
principle.

### Liskov Subsitute Principle

The principle states that objects of a superclasses can be replaced with the objects of the subclasses without breaking the application.
For this to happen , the subclass should at the least apply the same rules/operation to all the output parameters as applied by the parent class."Don’t implement any stricter validation rules on input parameters than implemented by the parent class"

### Interface Segregation Principle

"Clients should not be forced to depend upon interfaces that they do not use.”. If not followed then other classes would have to be modified 
for methods that are not even used by it.

### Dependency Inversion Principle

"High-level modules, which provide complex logic, should be easily reusable and unaffected by changes in low-level modules, which provide utility features. To achieve that, you need to introduce an abstraction that decouples the high-level and low-level modules from each other."


## References

* <https://stackify.com/solid-design-open-closed-principle/>
* <https://stackify.com/solid-design-principles/>
* <https://stackify.com/solid-design-liskov-substitution-principle/>
* <https://stackify.com/interface-segregation-principle/>
* <https://stackify.com/dependency-inversion-principle/>