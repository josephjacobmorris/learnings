# Gradle

## Features

- Build automation: Gradle is a powerful build automation tool that allows you to define and execute your build
  processes. It automates tasks such as compiling source code, running tests, packaging, and deploying applications.
- Declarative build scripts: Gradle uses a Groovy-based DSL (Domain-Specific Language) or Kotlin DSL to define build
  scripts. These scripts are declarative, allowing you to specify what needs to be done rather than how to do it, making
  build configuration more concise and maintainable.
- Dependency management: Gradle provides advanced dependency management capabilities. It can resolve dependencies from
  various sources such as local repositories, remote repositories, and version control systems. It supports transitive
  dependencies, conflict resolution, and dynamic versioning.
- Extensibility: Gradle is highly extensible, allowing you to customize and extend its functionality. You can write
  custom plugins, apply existing plugins, and leverage a wide range of community-developed plugins to meet your specific
  build requirements.
- Incremental builds: Gradle performs incremental builds by intelligently determining which parts of your project have
  changed since the last build. This results in faster build times, as only the modified parts need to be recompiled or
  processed.
- Multi-project support: Gradle supports building and managing multi-project builds. You can define hierarchies of
  projects with shared dependencies, resources, and tasks. This enables efficient modularization and reuse of common
  components.
- Task-based model: Gradle operates on a task-based model, where you define tasks that perform specific actions such as
  compiling code, running tests, generating documentation, or deploying artifacts. You can configure dependencies
  between tasks and define custom tasks to automate any desired action.
- Incremental task execution: Gradle's incremental task execution executes only the necessary tasks based on their
  inputs and outputs. This feature improves build performance by skipping tasks that don't need to be rerun.
- Integration with IDEs: Gradle integrates well with popular IDEs such as IntelliJ IDEA, Eclipse, and Android Studio. It
  provides plugins and tooling support that enables seamless integration with the development environment, making it
  easier to build, test, and debug your projects.
- Continuous integration support: Gradle works well with continuous integration (CI) systems like Jenkins, Travis CI,
  and TeamCity. It provides plugins and features to facilitate smooth integration into CI pipelines, including test
  reporting, artifact publishing, and build result notifications.

## Gradle Wrapper

The Gradle Wrapper is a tool bundled with Gradle that allows developers to specify a specific version of Gradle to be
used for their project. It ensures that the required Gradle version is automatically downloaded and used, providing a
consistent and reproducible build environment across different machines.

## Gradle Commands

| Command        | Description                     |
|:---------------|---------------------------------|
| `gradle init`  | Create new gradle project       |
| `gradle tasks` | List all available gradle tasks |

## Different Components of build.grdle

The `build.gradle` file is the build script in Gradle that defines the configuration and tasks for your project. It
consists of different components that serve specific purposes. Here are the key components and their functionalities:

### 1. Plugins:

In Gradle, plugins are a fundamental component of the build system that extend the functionality and provide predefined
tasks, configurations, and conventions for your project. Plugins enable you to easily incorporate common build features
without having to write custom code. Here's a more in-depth explanation of plugins in the `build.gradle` file, along
with examples:

1. Applying Plugins:
   To apply a plugin, you use the `plugins` block in your `build.gradle` file. There are two main ways to apply a
   plugin:

   a. Using the plugin ID:
   ```groovy
   plugins {
       id 'java'
   }
   ```
   This applies the built-in `java` plugin, which adds Java-specific build tasks and configurations to your project.

   b. Using the plugin ID and version:
   ```groovy
   plugins {
       id 'com.github.johnrengelman.shadow' version '7.0.0'
   }
   ```
   This applies the external `shadow` plugin with a specific version, which is commonly used for creating fat JARs.

2. Built-in Plugins:
   Gradle provides a set of built-in plugins that cover various common scenarios, such as building Java projects,
   generating documentation, or working with version control systems. These plugins offer a wide range of tasks and
   conventions out-of-the-box, making it easier to build projects in those domains.

   For example, the `java` plugin adds tasks like `compileJava`, `test`, and `jar` to compile Java source code, run
   tests, and create a JAR file respectively.

3. External Plugins:
   Apart from the built-in plugins, Gradle allows you to use external plugins to extend the build system's capabilities.
   External plugins are usually distributed through public repositories like the Gradle Plugin Portal or other community
   repositories.

   For instance, the popular `jacoco` plugin can be applied as follows:
   ```groovy
   plugins {
       id 'jacoco'
   }
   ```
   The `jacoco` plugin enables code coverage reporting in your project.

4. Custom Plugins:
   Gradle provides a mechanism to create custom plugins tailored to your specific needs. You can write your own plugin
   code and apply it in the `build.gradle` file or include it as a separate plugin module.

   Here's an example of a custom plugin applied using a plugin classpath:
   ```groovy
   plugins {
       id 'com.example.myplugin' version '1.0.0' apply false
   }

   buildscript {
       repositories {
           mavenCentral()
       }
       dependencies {
           classpath 'com.example:myplugin:1.0.0'
       }
   }

   apply plugin: 'com.example.myplugin'
   ```
   This example applies a custom plugin named `myplugin` with version `1.0.0`, which is resolved from the specified
   Maven repository.

Plugins are a powerful way to extend and customize the behavior of the Gradle build system. By applying plugins, you can
leverage pre-built functionality, tasks, and configurations to simplify your build process and improve productivity.

### 2. Dependencies:

Dependencies specify external libraries or modules required by your project. They can be fetched from repositories. For
example:

   ```groovy
   dependencies {
    implementation 'com.google.guava:guava:30.1-jre'
    testImplementation 'junit:junit:4.13.2'
}
   ```

Here, the `guava` library is added as a runtime dependency, while `junit` is a test dependency.

### 3. Task Definitions:

Tasks define specific actions or operations to be executed during the build process. For example:

   ```groovy
   task clean(type: Delete) {
    delete 'build'
}

task compileJava(type: JavaCompile) {
    sourceSets.main.java.srcDirs = ['src']
    classpath = sourceSets.main.compileClasspath
    destinationDir = file('build/classes')
}
   ```

The `clean` task deletes the build directory, while `compileJava` compiles the Java source code.

### 4. Configurations:

In Gradle's `build.gradle` file, configurations define sets of dependencies for different purposes, such as compile,
runtime, or test. They allow you to group dependencies and manage their scopes. Here's an in-depth explanation of
configurations with examples:

1. Default Configurations:
   Gradle provides several default configurations, including:
    - `implementation`: Dependencies required for compiling and running your application.
    - `compileOnly`: Dependencies needed only during compilation and not at runtime.
    - `runtimeOnly`: Dependencies required only at runtime, not for compilation.
    - `testImplementation`: Dependencies required for testing.
    - `testRuntimeOnly`: Dependencies needed only during testing at runtime.

   Example:
   ```groovy
   dependencies {
       implementation 'com.google.guava:guava:30.1-jre'
       testImplementation 'junit:junit:4.13.2'
   }
   ```

2. Custom Configurations:
   You can define your own configurations to group dependencies based on your project's needs. For example, you might
   create a configuration for specific modules or plugins.

   Example:
   ```groovy
   configurations {
       myConfiguration
   }
   
   dependencies {
       myConfiguration 'com.example:custom-module:1.0.0'
   }
   ```

3. Extending Configurations:
   You can extend or inherit configurations to include additional dependencies or override existing ones. This is useful
   when you want to add specific dependencies to a default configuration.

   Example:
   ```groovy
   configurations {
       runtimeClasspath {
           extendsFrom implementation
           // Additional dependencies for runtimeClasspath
       }
   }
   ```

4. Configuration Resolution:
   Gradle resolves configurations by retrieving dependencies from repositories. It searches for the requested
   dependencies based on the configuration's scope and resolves transitive dependencies automatically.

5. Configuration Usage:
   Once you define a configuration, you can use it to declare dependencies within the `dependencies` block.

   Example:
   ```groovy
   dependencies {
       myConfiguration 'com.example:custom-module:1.0.0'
   }
   ```

   Here, `myConfiguration` is used to specify the dependency on the custom module.

6. Configuration Visibility:
   Configurations can have different visibility scopes. By default, they are visible to the entire project. However, you
   can limit their visibility to specific subprojects or even make them private.

   Example:
   ```groovy
   configurations {
       myConfiguration {
           visible = false
       }
   }
   ```

   In this case, `myConfiguration` is not visible to other subprojects or build scripts.

Configurations in Gradle provide a flexible way to manage dependencies and control their scopes. They allow you to
organize your dependencies based on different purposes, extend or override existing configurations, and create custom
configurations to suit your project's needs.

### 5. Customizations:

You can define custom configurations, properties, and rules as per your project requirements. For example:

   ```groovy
   repositories {
    mavenCentral()
}

sourceCompatibility = 11
version = '1.0.0'

tasks.withType(JavaCompile) {
    options.encoding = 'UTF-8'
}
   ```

Here, the `repositories` block specifies the Maven repository, `sourceCompatibility` sets the Java version, `version`
defines the project version, and the `JavaCompile` rule sets the compilation encoding.

### 6. Repositories :

In Gradle, repositories are used to specify where the build system should look for dependencies (external libraries or
modules) required by your project. Repositories provide access to these dependencies by hosting and serving the required
artifacts. There are different types of repositories, such as local repositories, remote repositories, and version
control repositories.

Here's a detailed explanation of repositories in the `build.gradle` file with examples:

1. Local Repositories:
   Local repositories are directories on your local machine where you can store and access dependencies. They can be
   useful for hosting custom or locally-built artifacts. By default, Gradle includes the local Maven
   repository (`~/.m2/repository`). You can also define additional local repositories. Example:

   ```groovy
   repositories {
       mavenLocal()
       flatDir {
           dirs 'libs'
       }
   }
   ```

   In this example, the `mavenLocal()` method adds the default local Maven repository, and the `flatDir` block specifies
   a local directory called 'libs' as a repository.

2. Remote Repositories:
   Remote repositories are servers or online locations that provide access to pre-built dependencies. The most commonly
   used remote repository is Maven Central. You can define multiple remote repositories in your `build.gradle` file.
   Example:

   ```groovy
   repositories {
       mavenCentral()
       jcenter()
       maven {
           url "https://example.com/repository"
       }
   }
   ```

   Here, `mavenCentral()` and `jcenter()` add the standard remote repositories, while the `maven` block defines a custom
   remote repository with the specified URL.

3. Repository Credentials:
   If your remote repository requires authentication, you can provide the necessary credentials. Example:

   ```groovy
   repositories {
       maven {
           url "https://example.com/repository"
           credentials {
               username 'myUsername'
               password 'myPassword'
           }
       }
   }
   ```

   In this example, the `credentials` block is used to specify the username and password required to access the remote
   repository.

4. Repository Order:
   Gradle searches for dependencies in repositories in the order they are defined in the `repositories` block. It stops
   searching as soon as it finds the required dependency. You can control the order by specifying repositories at
   different positions. Example:

   ```groovy
   repositories {
       mavenLocal()
       mavenCentral()
   }
   ```

   Here, Gradle first checks the local repository and then falls back to the central Maven repository if the dependency
   is not found locally.

5. Other Repository Types:
   Gradle also supports additional repository types, such as Ivy repositories, URL-based repositories, and repository
   mirroring. These provide flexibility for specific use cases. Example:

   ```groovy
   repositories {
       ivy {
           url "https://example.com/ivy-repo"
           layout 'pattern', {
               artifact '[organisation]/[module]/[revision]/[artifact]-[revision].[ext]'
           }
       }
       maven {
           url "https://example.com/repository"
       }
       maven {
           url "https://another-example.com/repository"
           metadataSources {
               mavenPom()
               artifact()
           }
       }
   }
   ```

   In this example, an Ivy repository and two Maven repositories with custom configurations are defined.

By configuring repositories in your `build.gradle` file, you ensure that Gradle knows where to look for the dependencies
your project requires. This allows the build system to automatically fetch and resolve dependencies from the specified
repositories, making them available for use in your project.

## Gradle build Phases

1. Initialization: In this phase, Gradle evaluates the build script and sets up the project's configuration. It applies
   plugins, defines tasks, and establishes the build environment.

2. Configuration: During this phase, Gradle configures the project and its tasks based on the defined settings, plugins,
   and dependencies. It determines task properties and relationships without executing the tasks.

3. Execution: The execution phase is where the actual tasks are executed. Gradle determines the task order based on
   dependencies and executes them accordingly. Tasks perform specific actions such as compiling code or running tests.

4. Finalization: In the finalization phase, Gradle performs cleanup activities, releases resources, and carries out
   post-processing tasks before completing the build.

Understanding these phases helps you grasp the sequence of events in a Gradle build, from initialization and
configuration to task execution and finalization.
These are some of the main components in a `build.gradle` file. They allow you to define and configure the build
process, manage dependencies, and customize the build according to your project's requirements.

## Groovy QuickStart

### DataTypes

Groovy uses java wrapper classes as datatypes

```groovy
a = 'string'
groovyString = "some value ${a}"
multiLineString = """
m
u
l
"""
def c = 3.14F
println a
println a.class
println groovyString
```

### Closures

Closures are similar to java lambdas

```groovy
def closure = { name ->
    println "Hello, ${name}!"
}

closure("John") // Output: Hello, John!

```

closures with it

```groovy
//in groovy it can be used as shorthand for a single parameter
def names = ["Alice", "Bob", "Charlie", "Dave"]

def result = names.findAll {
    it.length() > 4
}

println result

```

### Collection Types

* #### List

```groovy
list = [1, 3, 4]
```

* #### Set

```groovy
set = [1, 2, 2, 3] as set
```

* #### Map

```groovy
def map = [name: "John", age: 30]
println map // Output: [name:John, age:30]

println map.size() // Output: 2
println map.get("name") // Output: John
println map.name // Output: John
println map["name"] // Output: John

map.put("country", "USA")
println map // Output: [name:John, age:30, country:USA]

map.remove("age")
println map // Output: [name:John, country:USA]
```

### Methods

In groovy last line in a method is treated as return statement by default.

Method calls does'nt require enclosing parameters in ()

Datatypes for input parameters and return type is optional

```groovy
def greet(name) {
    return "Hello, ${name}!"
}

println greet("John") // Output: Hello, John!
```

#### Times

the times method is a convenient way to execute a block of code a specified number of times. It is available on any
numeric value (e.g., integers) and takes a closure as an argument. Here's an example:

```groovy
3.times {
    println "Hello, World!"
}

```

### Classes and Objects

In Groovy a default constructor and a constructor which accepts a map is created by default on creating the class.
`object.property=1` is actually calling the setter in groovy

In Groovy, classes define the structure of objects using the `class` keyword. Objects are instances of those classes,
created with the `new` keyword. Groovy automatically generates setters and getters for class properties, allowing easy
access and modification of property values. Here's an example:

```groovy
class Person {
    String name
    int age

    void sayHello() {
        println "Hello, my name is ${name} and I am ${age} years old."
    }
}

def person = new Person(name: "John", age: 30)
println person.name // Output: John
println person.age // Output: 30
person.sayHello()
```

In this concise example, we define a `Person` class with properties `name` and `age`. We create an object `person` with
the property values set directly. We access the property values using direct property access. The `sayHello()` method is
invoked on the object to print a greeting.

Using classes and objects in Groovy provides a way to structure and work with data in an object-oriented manner, while
automatic generation of setters and getters simplifies property access and modification.

## Understand Build.gradle

The `build.gradle` is used to create a `Project` object in gradle with
methods `repositories`, `dependencies` , `application` etc which accept closures.
The `plugins` is a block of code.

### project properties

Project properties are variables that can be defined and used within your build script. They allow you to configure and
customize your build based on specific values.

| Property Name                 | Default Value             | Description                                                                                                                  |
|-------------------------------|---------------------------|------------------------------------------------------------------------------------------------------------------------------|
| `project.name`                | Project name              | The name of the project. It can be customized as needed.                                                                     |
| `project.version`             | Unspecified               | The version of the project. Typically follows semantic versioning.                                                           |
| `project.group`               | Unspecified               | The group or organization to which the project belongs.                                                                      |
| `project.description`         | Unspecified               | A brief description of the project.                                                                                          |
| `project.buildDir`            | `${rootProject.buildDir}` | The directory where build outputs, such as compiled classes and generated artifacts, are stored.                             |
| `project.sourceCompatibility` | Java version of Gradle    | The source compatibility version for the project's source code. By default, it uses the Java version configured in Gradle.   |
| `project.targetCompatibility` | Java version of Gradle    | The target compatibility version for the project's compiled code. By default, it uses the Java version configured in Gradle. |
| `project.repositories`        | Unspecified               | The repositories used for dependency resolution in the project. By default, it uses the repositories configured in Gradle.   |
| `project.dependencies`        | Unspecified               | The dependencies required by the project. By default, it uses the dependencies configured in Gradle.                         |
| `project.tasks`               | Unspecified               | The tasks defined for the project. By default, it uses the tasks defined in Gradle.                                          |

These are just a few examples of project properties commonly used in Gradle build scripts. You can define and customize
additional properties based on your project's specific requirements. Project properties provide flexibility and
configurability, allowing you to tailor your build process and dependencies to suit your needs.

### Custom Project Properties

In Gradle, you can create and retrieve custom project properties by defining them in your build script or in a separate
properties file. Here's a short example to demonstrate how to create and retrieve custom project properties:

1. Defining Custom Project Properties:
   In your build.gradle file, you can define custom properties using the `ext` (extension) block. Here's an example:

```groovy
ext {
    customProperty = "Custom Value"
    customVersion = "1.0.0"
}
```

In this example, we define two custom project properties: `customProperty` and `customVersion`. You can assign any value
to these properties as per your requirements.

2. Retrieving Custom Project Properties:
   To retrieve the custom project properties, you can simply refer to them by name within your build script. Here's an
   example:

```groovy
task printCustomProperties {
    doLast {
        println "Custom Property: ${customProperty}"
        println "Custom Version: ${customVersion}"
    }
}
```

In this example, we define a custom task `printCustomProperties` that prints the values of the custom project
properties. When you execute this task, it will retrieve and display the values assigned to `customProperty`
and `customVersion`.

### Tasks
In Gradle, tasks are a fundamental building block of the build process. They represent units of work that need to be performed during the build and can include activities such as compiling code, running tests, packaging artifacts, deploying, and more. Tasks are organized within the Project Object Model (POM) of Gradle.

The Project Object Model (POM) is a hierarchical structure that represents the project and its dependencies. It consists of projects, configurations, tasks, and other elements. Tasks are associated with specific projects and can have dependencies on other tasks or be dependent on them. Tasks are defined and configured using the Groovy or Kotlin DSL (Domain-Specific Language) in the build script.

Here are some common task methods in Gradle along with short code examples:

1. `doFirst` and `doLast`: These methods allow you to specify actions to be executed before or after the task's main action.
```groovy
task myTask {
    doFirst {
        println "Executing first action"
    }
  
    doLast {
        println "Executing last action"
    }
}
```

2. `dependsOn`: This method allows you to define dependencies between tasks. It ensures that the specified tasks are executed before the current task.
```groovy
task taskA {
    doLast {
        println "Executing Task A"
    }
}

task taskB {
    doLast {
        println "Executing Task B"
    }
}

task taskC {
    dependsOn taskA, taskB
    doLast {
        println "Executing Task C"
    }
}
```

3. `onlyIf`: This method allows you to conditionally execute a task based on a given condition.
```groovy
task myTask {
    def shouldExecute = true

    onlyIf {
        shouldExecute
    }
  
    doLast {
        println "Executing My Task"
    }
}
```

4. `inputs` and `outputs`: These methods define the inputs and outputs of a task. Gradle uses these to determine if a task is up-to-date and needs to be executed.
```groovy
task myTask {
    inputs.file 'input.txt'
    outputs.file 'output.txt'
  
    doLast {
        println "Executing My Task"
    }
}
```

## References

* Chatgpt