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

## References

* https://stackoverflow.com/questions/12206600/how-to-speed-up-insertion-performance-in-postgresql
* https://www.timescale.com/blog/real-time-analytics-in-postgres-why-its-hard-and-how-to-solve-it/
* [Explain On Delete Set Null](https://chat.openai.com/share/5bac6472-4058-40df-8e84-df4c82b9b5e1)
* [TOAST](https://chat.openai.com/share/0551b942-da8d-4229-9449-ce50cfb9c724)
* [Shared Memory in Postgresql](https://chat.openai.com/share/1551290b-edc0-4f94-a34f-35d20afc7541)
* [Two Phase Commit](https://chat.openai.com/share/1b8fbceb-663d-437d-adcc-2d1cbad08acb)
* [PUBLICATION](https://chat.openai.com/share/1bba311f-bc82-427e-aa89-b09dca94b512)
* Mastering PostgreSQL 15 