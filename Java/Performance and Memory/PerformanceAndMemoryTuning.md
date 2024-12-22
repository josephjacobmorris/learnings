# Performance and Memory Fine-Tuning
## Introduction
When we use `javac` to compile java code it coverts it to byte code and JVMs actually run the byte code (that's the reason why JVM can actually run multiple languages)

### JIT
The Java Virtual Machine (JVM) uses Just-in-Time (JIT) compilation to speed up execution by compiling bytecode to native machine code. This process involves the JVM monitoring which branches of code are executed most often and then deciding whether to compile specific methods or parts of methods into native machine code.

When JIT compilation is in use, some application code runs in interpreted mode as bytecode, while other code runs natively on the underlying hardware. The JVM can switch between these two modes seamlessly, allowing for faster execution times without requiring significant changes to the application code.

The process involves multiple steps:

1. The JVM monitors bytecode execution.
2. When a method is executed frequently, it is compiled into native machine code.
3. The JVM switches from interpreted mode to natively compiled mode only when necessary.

JIT compilation provides several benefits, including faster execution times and improved performance in applications with high CPU-intensive processing requirements. However, there are also limitations and considerations, such as potential performance reductions during JIT compilation and the need for careful evaluation of when to assess code performance.


## Design Time Optimization
### Stack
* There is multiple stack .i.e one stack is assigned per thread
* Local variables are stored inside stack (primitives are stored directly in stack while objects are stored in heap with reference stored in stack)
* Since stack uses the stack datastructure we have variable scoping rules in java.
* Each stack size is much smaller than the heap size

### Heap
* Heap stores all objects 
* It is shared across all threads.

> Note: Based on above, 
> 1) the `final` keyword in java ensures that value in stack cant change meaning the reference for objects and value  for primitives
> 2) the objects are passed by copying the value of reference while the actual object actually resides in the heap.
### Metaspace
Metaspace is a memory area in the Java Virtual Machine (JVM) introduced in Java 8 to replace the Permanent Generation (PermGen) space. It stores class metadata, such as:
**Class definitions**: Information about the structure of classes, their methods, fields, and inheritance relationships.
Method metadata: Bytecode instructions, method signatures, and other details about methods.
**Static Variables**
Key points about Metaspace:
Native memory:
Unlike the heap, Metaspace resides in native memory, which means it is outside the Java heap and is managed by the operating system.
Dynamic sizing:
Metaspace can dynamically grow and shrink depending on the application's needs, unlike PermGen which had a fixed size.
Garbage collection:
Metaspace is subject to garbage collection, meaning that unused class metadata can be reclaimed to free up memory.

### Escaping References

* using iterator
* copying the collections
* using immutable collections
* using copy constructor
* using interface to create immutable objects

### Interning Strings
String `intern()` ensures that for the same value of string same object is re-used. It is useful when there are strings calculated and having the same value.

### Garbage collection
Any object which cannot be reached from stack,metaspace will be eligible for garbage collected.
`System.gc()` is a hint to JVM to do garbage collection
## Runtime Optimization
### JVM Params
|               Flag                |                                                                                            Description                                                                                            |
|:---------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
|      `-XX:+PrintCompilation`      |                                                                               Print which code is natively compiled                                                                               |
|       `-XX:+PrintCodeCache`       |                                                                                     Print the code cache info                                                                                     |
|  `-XX:InitialCodeCacheSize=128k`  |                                                                                  Set the initial code cache size                                                                                  |
|  `-XX:ReservedCodeCacheSize=1m`   |                                                                               Set the max value of code cache size                                                                                |
| `-XX:CodeCacheExpansionSize=128k` |                                                                         How much to increment by when code cache is full                                                                          |
|      `-XX:+PrintFlagsFinal`       |                                                                        Prints all the jvm flags even those default values                                                                         |
|      `-XX:+PrintCompilation`      |                                                                                     Prints compiled code info                                                                                     |
|      `-XX:CICompilerCount=2`      | Number of threads used for compiling. In bigger application increasing number of threads might speed it up. In 32 bit java min is 1 whereas in 64-bit it is 2 (one for server and one for client) |
|     `-XX:CompilerThreshold=2`     |                                                          Number of times the method has to be run before it is considered for compiling.                                                          |
| `-XX:+PrintStringTableStatistics` |                                                                                 Prints jvm string pool statistics                                                                                 |
|   `-XX:StringTableSize=120121`    |                                                                Increases the string pool bucket size and hence may improve latency                                                                |
|      `-XX:MaxMetaspaceSize`       |                                                                               control the maximum size of Metaspace                                                                               |
|             `-client`             |                                                         Runs the jvm in client mode which is more memory efficient. Has less startup time                                                         |
|              `-Xms`               |                                                           Initial heap size. Give sufficient initial heap size can speedup startup time                                                           |
|              `-Xmx`               |                                                                                        Maximum heap size.                                                                                         |
## References
* Udemy - Discover how coding choices, benchmarking, performance tuning and memory management can optimize your Java applications
* LLMs
* https://stackoverflow.com/questions/10578984/what-is-java-string-interning
* https://www.geeksforgeeks.org/interning-of-string/