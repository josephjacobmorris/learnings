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

## JVM Params
|               Flag                |                                                                                            Description                                                                                            |
|:---------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
|      `-XX:+PrintCompilation`      |                                                                               Print which code is natively compiled                                                                               |
|       `-XX:+PrintCodeCache`       |                                                                                     Print the code cache info                                                                                     |
|  `-XX:InitialCodeCacheSize=128k`  |                                                                                  Set the initial code cache size                                                                                  |
|  `-XX:ReservedCodeCacheSize=1m`   |                                                                               Set the max value of code cache size                                                                                |
| `-XX:CodeCacheExpansionSize=128k` |                                                                         How much to increment by when code cache is full                                                                          |
|      `-XX:+PrintFlagsFinal`       |                                                                        Prints all the jvm flags even those deflaut values                                                                         |
|      `-XX:+PrintCompilation`      |                                                                        Prints all the jvm flags even those deflaut values                                                                         |
|      `-XX:CICompilerCount=2`      | Number of threads used for compiling. In bigger application increasing number of threads might speed it up. In 32 bit java min is 1 whereas in 64-bit it is 2 (one for server and one for client) |
|     `-XX:CompilerThreshold=2`     |                                                          Number of times the method has to be run before it is considered for compiling.                                                          |
|      `-XX:+PrintCompilation`      |                                                                        Prints all the jvm flags even those deflaut values                                                                         |
|             `-client`             |                                                         Runs the jvm in client mode which is more memory efficient. Has less startup time                                                         |
## Design Time Optimization

## Runtime Optimization

## References
* Udemy - Discover how coding choices, benchmarking, performance tuning and memory management can optimize your Java applications
* LLMs