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
`System.gc()` is a hint to JVM to do garbage collection.

Garbage collection in java follows a mark and sweep process. JVM first checks which all objects are referenced from the stack and its children.
After marking them and moving them together the rest of objects are cleaned. Now this approach would be okay if most of the objects are to be garbage collected but
if there  are a lot of the long-lived objects this is not efficient . So the heap is divided into new and old . Usually the gc happens only in the new space and that is
called a minor gc . Usually gc happens in old space only if the heap is almost completely used and it is called a major GC.

The new gc is further divided into eden,s0 & s1. All the new objects are created in eden space after a gc if it survives it is moved to s0 and in further rounds all objects are moved to s1.
This means that at any given point s0 or s1 is not used simultaneously.
> Note: Usually a soft leak can be seen as a steep curve in old gen

#### finalise()
> Note: Depreciated since java 9

It is executed when object is destroyed during gc. It had a lot of inherent issues like infinite loops, memory leaks ,deadlocks etc
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
| `-XX:+HeapDumpOnOutOfMemoryError` |                                                                                       Does heap dump on OOM                                                                                       |
|        `-XX:HeapDumpPath`         |                                                                            Path to which heap dump file is to be saved                                                                            |
|   `-XX:StringTableSize=120121`    |                                                                Increases the string pool bucket size and hence may improve latency                                                                |
|      `-XX:MaxMetaspaceSize`       |                                                                               control the maximum size of Metaspace                                                                               |
|             `-client`             |                                                         Runs the jvm in client mode which is more memory efficient. Has less startup time                                                         |
|              `-Xms`               |                                                           Initial heap size. Give sufficient initial heap size can speedup startup time                                                           |
|              `-Xmx`               |                                                                                        Maximum heap size.                                                                                         |
|           `-verbose:gc`           |                                                                                          prints gc info                                                                                           |
|   `-XX:-UseAdaptiveSizePolicy`    |                                                                      turn off dynamic sizing of different components of heap                                                                      |
|         `-XX:NewRatio=2`          |                                                                        keeps the ratio of size between new and old gen 1:2                                                                        |
|       `-XX:SurvivorRatio=2`       |                                                                    keeps the ratio of size between survivor and eden space 1:2                                                                    |
|   `-XX:MaxTenuringThreshold=15`   |                                                              number of times it must survives b4 it is moved to old gen . Max is 15                                                               |
|        `-XX:+UseSerialGC`         |                                                                                           use serial gc                                                                                           |
|       `-XX:+UseParallelGC`        |                                                                                          use parallel gc                                                                                          |
|     `-XX:+UseConcMarkSweepGC`     |                                                                                         use concurrent gc                                                                                         |
|          `-XX:+UseG1GC`           |                                                                                         use concurrent gc                                                                                         |
| ` -XX:+UseZGC -XX:+ZGenerational` |                                                                                  use concurrent generational zgc                                                                                  |
|   `-XX:UseStringDeDuplication`    |                                          available in g1gc only. if non string pool strings are having same value and useful in a memory constrained env                                          |
|  `-XX:StartFlightRecording=...`   |                                                                          to configure start of flight recording in java                                                                           |

### Memory Leaks
#### JVisualVM
Can be used to see memory growth
#### Heap Dump

### Performance Analysis
#### JMC - Java Mission Control

#### JFR - Java Flight Recorder

#### JMH - Java Micro benchmarking Harness

## Coding Choices

### List

#### ArrayList
#### CopyOnWriteArrayList
Use when:
* Multi-threaded application with many threads accessing the sam list
* Lots of read and very few writes
#### LinkedList
Sorting is slightly slower in linkedlist than in ArrayList
#### AttributeList
Child of ArrayList. Used for mbean
#### RoleList
Child of ArrayList. Used for managing role objects
#### RoleUnresolvedList
Child of ArrayList. Used for managing role objects
#### Stack
Child object of vector. Recommended to use LinkedList instead
#### Vector
Thread safe alternative to arraylist. Been there before arraylist.

### Map

#### HashMap
`hashCode()` method is used to convert non-integer values to int. HashMap works having an array and each element in the array is a linked list.
When the hashmap is about 3/4th (default actually depends on factor) full it is resized.High factor means less resizing.  

### LinkedHashMap
> note : no performance difference between hashmap and linked hashmap 

Items can be retrieved in the their defined order. Requires more memory than hashmap.

### Hashtable
Threadsafe version

### TreeMap
Sorted keys

## Other Coding Choices
### Primitives vs Object.
- Primitives are faster for most operations including accessing, mathematical operations since they live on stack.
- Primitives occupy  less memory compared to their wrapper counterparts
### BigDecimal vs Double
### StringBuilder vs Concatenating Strings
### Loops vs Streams
## References
* Udemy - Discover how coding choices, benchmarking, performance tuning and memory management can optimize your Java applications
* LLMs
* https://stackoverflow.com/questions/10578984/what-is-java-string-interning
* https://www.geeksforgeeks.org/interning-of-string/
* https://inside.java/2023/11/28/gen-zgc-explainer/
* https://medium.com/@roopa.kushtagi/netflixs-journey-from-g1gc-to-zgc-tackling-tail-latencies-66ee75681d49
* https://www.baeldung.com/jvm-series
* https://www.baeldung.com/jvm-garbage-collectors
* https://www.baeldung.com/java-21-generational-z-garbage-collector
* https://stackoverflow.com/questions/71674060/java-flight-recorder-continuous-rolling-recording
* https://www.baeldung.com/java-flight-recorder-monitoring
* https://www.baeldung.com/java-microbenchmark-harness
* https://github.com/openjdk/jmh
* https://medium.com/@truongbui95/jmh-java-microbenchmark-harness-tests-in-java-applications-f607f00f536d
* https://www.baeldung.com/java-primitives-vs-objects
* https://stackoverflow.com/questions/5199359/why-do-people-still-use-primitive-types-in-java