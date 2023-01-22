# Maven

## Plugins

### Releases using Maven

We can do releases using the maven release plugin

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-release-plugin</artifactId>
       <configuration>
         <goals>install</goals>
        <autoVersionSubmodules>true</autoVersionSubmodules>
       </configuration>
</plugin>
```
By default the goal is deploy, since in my use-case it was not required we are changing the goal to install and autoVersionSubmodules tag tells not to give seperate versions for submodules.