# Maven

## Plugins

### Test Report Generation

To generate the test report add the following plugin to the build section of your pom.xml file

```xml
<plugin>
    <groupId>org.jacoco</groupId>
        <artifactId>jacoco-maven-plugin</artifactId>
        <version>0.8.7</version>
        <executions>
            <execution>
                <goals>
                 <goal>prepare-agent</goal>
                </goals>
            </execution>
            <execution>
                <id>generate-code-coverage-report</id>
                <phase>test</phase>
                <goals>
                    <goal>report</goal>
                </goals>
            </execution>
        </executions>
</plugin>
```
