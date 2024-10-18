# Micronaut Flyway

## Basics usage
### Dependencies for micronaut flyway
```groovy
dependencies {
    implementation("io.micronaut.flyway:micronaut-flyway")
    implementation("io.micronaut.sql:micronaut-jdbc-h2") // Change to your preferred database driver
    // for mysql
    runtimeOnly("org.flywaydb:flyway-mysql")
    // for oracle
    runtimeOnly("org.flywaydb:flyway-database-oracle")
    // for postgres
    runtimeOnly("org.flywaydb:flyway-database-postgresql")
    // for sql server
    runtimeOnly("org.flywaydb:flyway-sqlserver")
}
```

###  Configure Flyway and Database Connection

In your `src/main/resources/application.yml`, configure Flyway and your database connection. Here’s an example configuration for an H2 in-memory database:

```yaml
flyway:
  locations: classpath:db/migration # Location of migration files
  user: your_db_user                 # Database user
  password: your_db_password         # Database password
  url: jdbc:h2:mem:default;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE

datasources:
  default:
    driverClassName: io.h2.Driver
    url: jdbc:h2:mem:default;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
    username: your_db_user
    password: your_db_password
```

### Create Migration Scripts

Create a directory for your migration scripts:
```bash
mkdir -p src/main/resources/db/migration
```

Then, create your migration files in this directory following Flyway’s naming convention. For example, create a file named `V1__Initial_setup.sql` with the following content:

```sql
CREATE TABLE example (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL
);
```

## Run Migrations Manually
**1** Disable automatic migration
```yaml
flyway:
  enabled: true
  datasources:
    default:
      enabled: false
```
**2** inject the bean FlywayMigrator and execute the `run` method

## Events

Micronaut events emitted
* SchemaCleanedEvent
* MigrationFinishedEvent

## Endpoint
micronaut provides builtin endpoint to expose all the applied migrations `/flyway`
It is disabled by default. To enable it add the config:
```yaml
endpoints:
  flyway:
    enabled: true
    sensitive: false
```

## References
https://micronaut-projects.github.io/micronaut-flyway/latest/guide/


