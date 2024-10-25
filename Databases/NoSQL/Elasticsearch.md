# Elasticsearch

## Optimizing Query Performance

### Preload data into file system cache
This is a detailed explanation of Elasticsearch's index.store.preload setting, which controls how the operating system caches I/O operations for the files associated with an index.

The setting accepts a comma-separated list of file extensions that will be preloaded upon opening. It can also be set at the index level using the `index.store.preload` field in Elasticsearch configuration files (e.g., `elasticsearch.yml`) or when creating the index at index creation time using the `PUT /my-index-000001 { "settings": { ... } }` method.

The default value for this setting is an empty array, indicating that no files will be loaded into the file system cache eagerly. However, this can lead to slower indexing and searching performance if the index is actively searched or large amounts of data are stored in the index.

There are several ways to customize the preload list, including:

* Using a wildcard `index.store.preload: ["*"]` for all files, which may not be suitable for certain types of fields.
* Setting it to a specific list of file extensions, such as `["nvd", "dvd", "tim", "doc", "dim"]`, for the most important parts of the index (e.g., norms, doc values, terms dictionaries).
* Using a vector-based preload setting, such as `index.store.preload: ["vec", "vex", "vem"]`, for search and aggregations.

It is essential to note that this setting can be dangerous if used on larger indices, as it would cause the file system cache to be trashed upon reopening after large merges. Therefore, it is crucial to carefully evaluate the trade-offs of using this setting in your specific use case.

## References
* https://www.elastic.co/guide/en/elasticsearch/reference/current/tune-for-search-speed.html
* https://www.elastic.co/guide/en/elasticsearch/reference/current/preload-data-to-file-system-cache.html