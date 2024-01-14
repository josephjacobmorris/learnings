# Postgres

## Performance Improvement

* Create parition in table
* `uuid_generate_v4()` over `gen_random_uuid()`

## Materialized Views
1. **Materialized Views**: Materialized views are a feature in PostgreSQL that allow you to pre-compute and store the results of complex, resource-intensive queries as tables. This can significantly improve query performance for analytics on large volumes of data.

2. **Limitations of Materialized Views**:
    - **Inefficient Refreshes**: When refreshing a materialized view, the entire dataset is recomputed, making refreshes computationally expensive and slow.
    - **Manual Refreshes**: Materialized views do not automatically update with new data; they require manual refreshes using the `REFRESH MATERIALIZED VIEW` command.
    - **Out-of-Date Results**: Materialized views may not include the latest data, resulting in out-of-date results.

3. **Continuous Aggregates**: TimescaleDB, an extension for PostgreSQL, introduces "continuous aggregates" as an enhancement to materialized views for real-time analytics. Continuous aggregates offer the following advantages:
    - **Automatic Refreshes**: You can define a refresh policy for continuous aggregates, and they are automatically and efficiently updated at specified intervals.
    - **Efficient Refresh Process**: Continuous aggregates update only the data that has changed since the last refresh, making the refresh process more efficient.
    - **Up-to-Date Results**: Continuous aggregates combine materialized data with new, not-yet-materialized data to provide up-to-date results.

4. **Usage Example**: The article provides an example of using continuous aggregates to power a real-time dashboard for a transportation agency. It shows how to create continuous aggregates with refresh policies to provide live, daily, and weekly overviews of key metrics.

5. **Hierarchical Continuous Aggregates**: TimescaleDB also introduces hierarchical continuous aggregates, which allow you to create continuous aggregates on top of other continuous aggregates, making the process even more efficient.

6. **Conclusion**: The article concludes that while PostgreSQL was not originally designed for applications that require processing large live datasets, materialized views are a powerful feature for improving query performance. However, they have limitations, and TimescaleDB's continuous aggregates address these limitations by providing automated, efficient, and up-to-date solutions for real-time analytics.

## Postgresql 15 features 

### DBA Related Features
* Support for `pg_dump` for older versions than 9.2 has been removed.
* Deprecating python2 in stored procedures 
* Fixed the public schema - Previously the public schema was available to every user. This was a security concern as one user could just revoke the access of other users. Now the `public` schema is not available by default. The user should be granted permission to use it.  
f* Supports adding permissions to variables.
* `pg_stat_statements` can now display info about the JIT compilation process and is helpful in detecting JIT related performance issues.
* New wait events - ???
* Adding new json logging features which enables logs to be parsed more easily.This can be done by modifying the `postgresql.conf`
<code>
`log_destination`= 'jsonlog'
`logging_collector`= on
</code>s

### Developer Related Features
* Security invoker views - ???
* ICU locales - The locale determines the sort order and in postgres the sort used to be provided by underlying  C library. The sort order changes over time and as indexes are basically sorted lists, this breaks indexing
* Numeric datatypes allow -ve scale `select 1234::numeric(5,-1);` gives 1230
* ON DELETE SET NULL behaviour in upon deletion marks the foreign key as null and hence maintaining the integrity of reference on delete. In Pg 15 it is now possible to set null only for few columns.
* Previously UNIQUE constraint used to treat null as different values but by adding `NULLS NOT DISTINCT` along with unique key will allow only one null entry
* `Merge` command is introduced in postgres

### Performance Related Features
*  Now possible to add compression algorithms for various things especially `TOAST ` TOAST stands for "The Oversizes-Attribute Storage Technique."
* 'Distinct' query uses parallelism as of pg 15
* Uses shared memory instead of UDP packets to transmit information.
* 2 Phase Commit is now supported for Logical Decoding
* Allows Column and row Level filtering for `publication`
* In previous versions postgresql was only able to compress at client side but in pg 15 it is now possible to compress at server side hence saving bandwidth.
* Introduced archival_library for easier backups.

## Transactions and Locking
> Note `Now()` returns the transaction time instead of actual time use `clock_timestamp()` to get real time

* If more than one statement has to be part of transaction then it should be enclosed by `BEGIN` .. `END`
* We can have unlimited `SAVEPOINT` inside the transaction. After transaction the savepoint wont exist and we can release a savepoint using `RELEASE SAVEPOINT name`.
* **TRANSACTIONAL DDLs** - In Postgresql even DDLs are transactional (except `DROP/CREAE DATABASE/TABLESPACE` etc) . So in case of version upgrades it ensures that the DDL changes are atomic.
* In case of concurrent read and write the read will return the old data and the write will update the data. This behaviour is called Multi-Version Concurrency Control (MVCC)
* In case of concurrent writes (updates) postgres guarantees that updates are done sequentially.

### Common Scenarios

   * **Explicit Table locking using lock table command**

     |           Lock           | Description                                                                                                                                                                                                     |
     |:------------------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
     |      `ACCESS SHARE`      | Used by read operations. Conflicts only with `ACCESS EXCLUSIVE`.                                                                                                                                                |
     |       `ROW SHARE`        | Used by `FOR UPDATE/FOR SHARE`. Conflicts with `ACCESS EXCLUSIVE` and `EXCLUSIVE`.                                                                                                                              |
     |     `ROW EXCLUSIVE`      | Used by Insert, Update, and Delete operations. Conflicts with `SHARE`, `SHARE ROW EXCLUSIVE`, `EXCLUSIVE`, and `ACCESS EXCLUSIVE`.                                                                              |
     |         `SHARE`          | Used by operations that don't modify data (e.g., SELECT ... FOR KEY SHARE). Conflicts with `ROW EXCLUSIVE`, `SHARE ROW EXCLUSIVE`, `EXCLUSIVE`, and `ACCESS EXCLUSIVE`.                                         |
     |  `SHARE ROW EXCLUSIVE`   | Used for creating index. Conflicts with `ROW EXCLUSIVE`, `EXCLUSIVE`, and `ACCESS EXCLUSIVE`.                                                                                                                   |
     |       `EXCLUSIVE`        | Prevents read and write . Most restrictive                                                                                                                                                                      |
     |    `ACCESS EXCLUSIVE`    | Prevents concurrent transactions from reading and writing                                                                                                                                                       |
     | `SHARE UPDATE EXCLUSIVE` | Used in the CREATE INDEX CONCURRENTLY, ANALYZE, ALTER TABLE, VACCUM. Conflicts with `SHARE`, `SHARE ROW EXCLUSIVE`,`SHARE UPDATE EXCLUSIVE`, `EXCLUSIVE`, `ROW SHARE`, `ROW EXCLUSIVE`, and `ACCESS EXCLUSIVE`. |

* **Check for locks**
```roomsql
Select pid,wait_event_type,wait_event, query from pg_stat_activity where datname= 'test';
```
* **Select data , do processing and update**
When we are doing select to do processing , followed by update use `FOR UPDATE` else two concurrent transactions might read at same time and cause undesired results. This causes the second select to wait. If we don't want to wait we could use `SELECT FOR UPDATE NOWAIT` which will error out instead of waiting
We could also set lock_timeout to avoid potential infinite delay in second transaction like below
```roomsql
SET lock_timeout TO 5000;
```
In scenarios similar to seat booking where we don't want the other customers to wait. We can skip the locked rows using `SELECT FOR UPDATE SKIP LOCKED`
`FOR UPDATE` also works on foreign keys and foreign keys are also locked. This might to lead to contentions.
Other locks which can be considered are `FOR NO KEY UPDATE`, `FOR SHARE` and `FOR KEY SHARE`.

### Transaction Isolation Levels
* READ COMMITTED
* REPEATABLE READ
* SERIALIZABLE SNAPSHOT ISOLATION (SSI)

### Advisory Locks
In Advisory locks what we are locking is a number and they dont unlock automatically on commit.
```roomsql
SELECT pg_advisory_lock(12);
....
COMMIT
SELECT pg_advisory_unlock(12);
```
Use `pg_advisory_unlock_all` to unlock all locked numbers

### Optimizing Storage/CleanUp
Delete cant clear data as ROLLBACK can't happen properly. So the solution is to use VACUUM.
VACUUM will map all the free space to the free space map (FSM) of the relation
> Note : VACCUM will not release the space in most cases and it only releases space if there are not valid rows at end of a table.

| Feature                     | VACUUM                     | VACUUM FULL                 |
|-----------------------------|----------------------------|-----------------------------|
| Reclaims storage            | Yes                        | Yes                         |
| Compacts table              | No                         | Yes                         |
| Releases space to OS        | No                         | Yes                         |
| Locking                     | Lightweight (concurrent)   | Exclusive                  |
| Downtime                    | Minimal                    | Potentially significant     |
| Frequency                   | Regularly used              | Occasionally used          |
| Concurrent with operations  | Yes                        | No                          |
| Use case                    | Routine maintenance        | Periodic, scheduled         |

Keep in mind that while `VACUUM FULL` is more aggressive in returning space to the operating system, it comes with the cost of potentially longer downtime and exclusive table locks. Therefore, its usage should be carefully considered and scheduled appropriately to minimize the impact on the application's availability.
> Note : Consider using `pg_squeeze` instead of VACUUM FULL to avoid blocking concurrent writes.

#### Configuring VACUUM and Autovacuum
* vacuum is done now automatically by tool autovacuum which is part of pg server. It wakes up once every minute (`autovacuum_naptime = 1`). If vacuum is required it(asks main process to) spins up three workers (based on `autovacuum_max_workers`).
* `autovacuum_vacuum_threshold=50` 
* `autovacuum_vacuum_scale_factor=0.2` 
* `autovacuum_analyze_threshold=50` 
* `autovacuum_analyze_scale_factor=0.1`
* In past (prior to postgres) autovacuum was not triggered for inserts but now it is triggered based on `autovacuum_vacuum_insert_threshold`
* Transaction IDs used for transactions are finite , this case also vacuum becomes relevant.

## Index & How to use them
* `\timing` in psql can be used to give runtime of the query (as measured by psql and not the server)
* Run `EXPLAIN` to identify the performance bottleneck in your query
    * `EXPLAIN` returns the penalty points and not the actual time it takes to execute the query
* The `pg_relation_size` function will give the size of the table in bytes.
* `\x` in psql enables the expanded display
* Indexing slows doing write operations. As of postgres version 11 we can use more cpu cores to build index
in parallel (only for B-tree index) via `max_parallel_maintenance_workers`
* B-tree indexes can also be used for sorting, finding min , max. They can be read in both ways high to low and vice-versa
* Postgres can use multiple indexes in the same query ( and same index also mutiple times as in the case of `OR` clause)
* Postgres will not use an index just bcoz one exists
* From version 13 , postgres has index deduplicated which occupies less storage,less ram and have higher cache hit rates
* `pg_stats` is a system view which contains all the stats of the column content.
* `CLUSTER` command helps in clustering data in table along a particular column.However
  * It locks table during clustering
  * can be done only based on one column
  * based on data changes the table might become declustered.
* Index Only Scan - If the result of the query is present in the index and then postgres does not read it from the disk
    * `INCLUDE` clause in index allows column to be present in the index which are not part of index keys
* Indexes can be defined on immutable functions
* Consider partial indexing wherever applicable.
* Modifying data while indexing is possible when we create index using `CREATE_INDEX CONCURRENTLY` command. But the index creation will be longer and it does'nt guarantee successful index creation.
* For date with special context postgres provides the feature to define operator classes

### Steps in Query Execution
* Parsing the query
* Rewriting the query to take care of rules
* The optimizer finds an ideal query plan
* The plan is then executed by the executor

### Index Types

| Index Type                     | Description                                                                                                     |      Use Case      |                                 Syntax                                 |
|:-------------------------------|:----------------------------------------------------------------------------------------------------------------|:------------------:|:----------------------------------------------------------------------:|
| B-tree                         |                                                                                                                 | >,<,<=,>,=,Max,Min |                                                                        |
| Heap                           |                                                                                                                 |                    |                                                                        |
| Hash                           | Not advisable before pg 10.0 since there was no WAL support. Hash indexes are larger than B-trees in most cases |         =          | `CREATE INDEX shorturl_hash_hash_ix ON shorturl_hash USING hash(key);` |
| GiST (Generalized Search Tree) | Can be used to implement r-tree behaviour, Fuzzy searching                                                      |                    |                                                                        |
| GIN (Generalized Inverted)     | Inverted index similar to elastic search.                                                                       |  Full text search  |                                                                        |
| BRIN (Block Range Indexes)     |                                                                                                                 |      >,<,<=,>      |                                                                        |

#### Hash Index
Hash index maintain hash of the key . It has less impact on insertion performance compared to b-tree and faster lookup (select) for unique values.
It has restrictions such as
* cannot enforce unique constraint
* cannot create index on multiple columns
* cannot cluster a table via hash index
* cannot create a sorted index
* cannot be used for range queries
* cannot be used by `ORDER BY` queries

#### BRIN (Block Range Indexes)
BRIN is index block and hence it occupies less space. It is recommended when your data is sorted

### Fuzzy Search
```roomsql
SELECT * FROM t_location
 ORDER BY name <-> 'Ktaly';
```

### Full Text Search
```roomsql
-- Search for documents containing the words 'food' or 'ice'
SELECT to_tsvector('english','I like food like ice-cream')  @@ to_tsquery('english', 'food | ice');
```

* `ts_debug` to understand why a search result eas returned.
* There are two options to define GIN indexes
  * have a column storing the vector in table and for update use trigger to maintain consistency, this occupies more space but better runtime behaviour
  * have functional index (with `to_tsvector` function) which is space efficient but slower

### Indexes for exclusion
Indexes can also be used for exclusion & data integrity.

## Advanced SQL - Aggregations,RECURSIONS,JSONB

### Range types
* Useful to store ranges has in-built check validation
* `daterange`, `int4range`

| Operator | Description               |
|:---------|:--------------------------|
| `<@`     | is part of range          |
| `&&`     | overlaps range            |
| `<<`     | strictly left of          |
| `&<`     | doesn not extend right of |
| `-\|-`   | is adjacent to            |
| `+`      | union                     |
| `*`      | intersection              |
| `*`      | difference                |

* As of postgresql 14, multiranges have also been added



### Recursions (Hierarchical Query)
Sample recursive query
```roomsql
with recursive x(n) as (
    SELECT 1 AS n, 'a'::text AS dummy // init condition
    UNION ALL
    SELECT n+2, dummy || 'a' //n +2 is the incremented value
    FROM x
    WHERE n <5 # loop condition
) 
SELECT * FROM x;
```
> Note:
> Using UNION instead of UNION ALL prevents calls with the same set of values. This can be useful to avoid infinite loops

### JSONB & JSON
* Use JSON only if we just want to store the `JSON` as text if it requires processing store it as `JSONB`
* The functions and operators only exist for `JSONB`

| Function/Operator      | Description                                                            |
|:-----------------------|:-----------------------------------------------------------------------|
| `jsonb_pretty`         | Beautify & format the json                                             | 
| `row_to_json`          | Pass anything to convert to json.Each row is coverted to seperate json | 
| `json_agg`             | Pass anything to convert to json.A single json document is returned    | 
| `json_populate_record` | Used to populate a row with json document                              | 
| `->`                   | Used to fetch subtree in json.Return type is 'jsonb'                   | 
| `jsonb_each`           | Used to loop over subtree and return all elements in composite type    | 
| `jsonb_each_text`      | Used to loop over subtree and return all elements as text              | 
| `jsonb_object_keys`    | Used to get the keys in json doc                                       | 



## References

* https://stackoverflow.com/questions/12206600/how-to-speed-up-insertion-performance-in-postgresql
* https://www.timescale.com/blog/real-time-analytics-in-postgres-why-its-hard-and-how-to-solve-it/
* [Explain On Delete Set Null](https://chat.openai.com/share/5bac6472-4058-40df-8e84-df4c82b9b5e1)
* [TOAST](https://chat.openai.com/share/0551b942-da8d-4229-9449-ce50cfb9c724)
* [Shared Memory in Postgresql](https://chat.openai.com/share/1551290b-edc0-4f94-a34f-35d20afc7541)
* [Two Phase Commit](https://chat.openai.com/share/1b8fbceb-663d-437d-adcc-2d1cbad08acb)
* [PUBLICATION](https://chat.openai.com/share/1bba311f-bc82-427e-aa89-b09dca94b512)
* [HASH INDEX](https://hakibenita.com/postgresql-hash-index)
* https://www.cybertec-postgresql.com/en/products/pg_squeeze/#:~:text=pg_squeeze%20is%20an%20open%20source,bloat%20%E2%80%93%20without%20extensive%20table%20locking.
* Mastering PostgreSQL 15 