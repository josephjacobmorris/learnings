# Spring Shell

## Introduction
**Spring Shell** is a Spring-based framework that allows developers to create interactive command-line applications with ease. It provides a way to define custom commands that can be executed from the terminal, making it useful for building CLI (Command Line Interface) tools. Spring Shell is commonly used to build developer tools, administration consoles, and interactive shells for applications.

### Key Features of Spring Shell:
1. **Command Creation**: You can define custom commands with annotations like `@ShellMethod`, and Spring Shell will handle parsing and execution.

2. **Input Parsing**: It automatically parses user input and maps it to method parameters in your commands.

3. **Auto-completion**: It supports tab completion for commands and arguments, making it user-friendly for interactive shell applications.

4. **Integration with Spring**: Since it's built on top of the Spring framework, it can easily integrate with other Spring components like dependency injection, Spring Boot, etc.

5. **Customizable Shell**: You can customize the shell's prompt, history, and more to fit your application's needs.

## Syntax

|      Annotation      |       Description        | Example |
|:--------------------:|:------------------------:|:-------:|
|      `@Command`      |         Used to          |         |
|  `@ShellComponent`   |         Used to          |         |
|    `@ShellMethod`    |         Used to          |         |
|      `@Option`       |                          |         |
| `@ExceptionResolver` | Used to handle exception |         |

### `@Command`
In **Spring Shell** (version 1.x), the `@Command` annotation was used to define methods that could be invoked as shell commands in a command-line interface (CLI). However, in later versions, specifically from **Spring Shell 2.x** onwards, `@Command` was replaced by `@ShellMethod` for the same purpose.

#### Key Features of `@Command` in Spring Shell 1.x:
- It was used to annotate a method in a class to make it executable as a shell command.
- Allowed customization of command names and arguments.

#### Example of `@Command` in Spring Shell 1.x:

```java
import org.springframework.shell.core.annotation.Command;
import org.springframework.stereotype.Component;

@Component
public class MyCommands {

    @Command(help = "Says hello to the user")
    public String hello(String name) {
        return "Hello, " + name;
    }
}
```

#### Command Execution:
If a user types the following in the command-line interface:

```bash
hello John
```

The output will be:

```bash
Hello, John
```

#### Transition to `@ShellMethod` (Spring Shell 2.x):
In newer versions of Spring Shell (from 2.x onwards), the `@Command` annotation was replaced by `@ShellMethod`. The functionality remains the same but with some enhancements. The new annotation simplifies the syntax and provides more flexible ways to define shell commands.

For example, the above `@Command` usage can be written as:

```java
import org.springframework.shell.standard.ShellComponent;
import org.springframework.shell.standard.ShellMethod;

@ShellComponent
public class MyCommands {

    @ShellMethod("Says hello to the user")
    public String hello(String name) {
        return "Hello, " + name;
    }
}
```

If you're using **Spring Shell 2.x or above**, use `@ShellMethod` instead of `@Command` since the latter is deprecated and no longer supported.

## References
* chatgpt
* https://docs.spring.io/spring-shell/reference/basics/index.html