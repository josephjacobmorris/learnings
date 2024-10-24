# Liquibase vs Flyway
## Similarities Between Liquibase and Flyway
Since Liquibase and Flyway implement the design principles of the evolutionary database they offer a lot of similar functionality. Both tools:

* Are open-source to an extent and help manage, track and deploy database schema changes.
* Use a versioned migration approach to a database schema change.
* Are based on Java and provides extensive support for Java frameworks like Spring Boot and Vert.x.
* Support integration with build tools like Maven and Gradle.
* Can run independently from the command line through provided scripts.
* Support a wide variety of databases.

## Differences Between Liquibase and Flyway

freestar
Flyway is built around a linear database versioning system that increments on each versioned change. This sometimes can create conflicts with parallel development. The filename of a Flyway script defines the type of migration. For example, a migration should have a convention of prefixes as V (for versioned), U (for undo), and R (for repeatable). It will be followed by a version number and a separator __(two underscores) followed by a description and a suffix .sql such as V01__Add_New_Column.sql.

Liquibase migration doesn’t need to follow any of the file name conventions. In Liquibase, changes are managed by one ledger known as a master changelog which will be defined as including all the migrations.

3.2. Storing a Change
Both the tools store the deployed change in a table. Flyway migrations are stored in the database schema with a default table named flyway_schema_history. Similarly, Liquibase stores its deployed migrations in a table named databasechangelog. Both the tools support overriding default configuration to change the table name.

3.3. Execution Order of a Change
Managing the order of a change is comparatively difficult in Flyway. With Flyway, the order depends on the version number and migration type in the filename. Contrarily, Liquibase uses a separate file named master_changelog in which the changes are deployed in the order they are defined.

3.4. Rollback a Change
Now, let’s discuss one of the main aspects of database migration. Rollback is needed whenever a bad change has caused a catastrophic issue in the application. Liquibase provides a way to roll back everything or undo specific migrations (available only on paid versions).


freestar
Flyway also has a undo migration, which can be deployed with a file name that starts with U followed by the version that needs to be undone. Its paid version also offers even more complex undo functionality.

Both the tools offer a decent rollback functionality, but considering only the free version, Flyway offers a good-to-use solution.

3.5. Selective Deployment of a Change
There are use cases where we need to deploy a change to only one environment. Liquibase wins here when we’ve to selectively deploy a change. Flyway is also capable of doing it, but you would have to set up a different configuration file for each environment or database. With Liquibase we can easily add labels and contexts to ensure deployment in certain places.

3.6. Java-Based Migration
Both tools are strongly Java Oriented and provide Java-based migration. Flyway and Liquibase allow defining a migration within a Java file. This can be advantageous in some scenarios.

3.7. Snapshots & Comparing Databases
Liquibase allows users to take a snapshot of the current state of the database. We can use this state to compare it to another database. This would be very helpful in scenarios like failover and database replication. Flyway on the other hand doesn’t support any of the snapshot features.


freestar
3.8. Conditional Deployment
Liquibase offers an added feature called pre-conditions. Preconditions allow users to apply changes based on the current state of the database. A changeset will only execute if it passes these preconditions.

Flyway on the other hand doesn’t support this. But through procedures, we can apply conditions in most SQL-based databases.


Here is a comparison between **Liquibase** and **Flyway** in a tabular format:

| Feature                              | **Liquibase**                                                                                                                                  | **Flyway**                                                                                                   |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| **Database Support**                 | Supports a wide variety of databases including SQL Server, MySQL, PostgreSQL, Oracle, and many more.                                           | Supports many major databases including PostgreSQL, MySQL, SQL Server, Oracle, etc.                          |
| **Migration Format**                 | XML, YAML, JSON, SQL                                                                                                                           | SQL, Java                                                                                                    |
| **Schema Versioning**                | Supports a detailed changelog file with each change described in a specific format (XML, SQL, etc.)                                            | Uses versioned SQL scripts with naming conventions (V1__Init.sql, V2__AlterTable.sql, etc.)                  |
| **Rollback Support**                 | Yes, supports rollback for both manual and automatic execution. Rollbacks can be defined for each changeSet. (available only on paid versions) | Supports undo scripts manually (as `U1__Undo.sql`), but more limited compared to Liquibase.                  |
| **Ease of Use**                      | Has a steeper learning curve due to the complex setup, but very powerful for large projects with complex changes.                              | Simple to use with straightforward SQL migrations, easier setup for smaller teams.                           |
| **Tracking Mechanism**               | Tracks schema changes using a changelog file and metadata tables in the database.                                                              | Tracks schema changes using metadata tables based on the applied migration scripts.                          |
| **Support for Non-SQL Changes**      | Supports complex changes such as stored procedures, triggers, and even non-SQL operations like file creation.                                  | Focuses mainly on database schema migrations and is SQL-centric. Java-based migrations are also possible.    |
| **Community and Support**            | Strong community and enterprise support. Large number of plugins and integrations.                                                             | Active community and broad usage, though smaller than Liquibase's. Offers commercial support as well.        |
| **Branching and Merging Support**    | Provides better support for handling branching and merging using "contexts" and "labels" to selectively apply migrations.                      | Lacks native branching and merging support; requires careful management when dealing with multiple branches. |
| **Extensibility**                    | Very extensible with support for custom preconditions, extensions, and a large plugin ecosystem.                                               | Supports Java-based custom migrations but is less extensible than Liquibase.                                 |
| **Execution Method**                 | Command-line, Maven, Gradle, Ant, Spring Boot integration, or embedded in applications.                                                        | Command-line, Maven, Gradle, Spring Boot integration, or embedded in applications.                           |
| **Changelog History**                | Yes, detailed history of changes in the changelog. Supports tracking of each change set individually.                                          | Stores history of applied migrations in the database, but each version is based on script names.             |
| **Preconditions Support**            | Yes, supports preconditions to ensure that certain criteria are met before a change is applied.                                                | No native preconditions support. Must manage manually through custom code or SQL.                            |
| **Testing and Validation**           | Provides command-line support for validation, diff, and status checks.                                                                         | Validates migrations at runtime, and checks applied vs unapplied migrations. Simple validation.              |
| **Learning Curve**                   | Steeper due to multiple migration formats and higher configurability.                                                                          | Easier due to simpler migration process, especially for SQL users.                                           |
| **License**                          | Open-source, with enterprise features available.                                                                                               | Open-source, with a commercial "Flyway Teams" edition available.                                             |
| **Selective Deployment of a Change** | Easy to add labels and contexts for selective deployment                                                                                       | Difficult to manage due to single table schema; can be set up for each environment or database               |


## References
* chatgpt
* https://www.baeldung.com/liquibase-vs-flyway