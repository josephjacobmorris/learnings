# Elasticsearch

## Optimizing Query Performance

### Preload data into file system cache
Elasticsearch's index.store.preload setting, which controls how the operating system caches I/O operations for the files associated with an index.

The setting accepts a comma-separated list of file extensions that will be preloaded upon opening. It can also be set at the index level using the `index.store.preload` field in Elasticsearch configuration files (e.g., `elasticsearch.yml`) or when creating the index at index creation time using the `PUT /my-index-000001 { "settings": { ... } }` method.

The default value for this setting is an empty array, indicating that no files will be loaded into the file system cache eagerly. However, this can lead to slower indexing and searching performance if the index is actively searched or large amounts of data are stored in the index.

There are several ways to customize the preload list, including:

* Using a wildcard `index.store.preload: ["*"]` for all files, which may not be suitable for certain types of fields.
* Setting it to a specific list of file extensions, such as `["nvd", "dvd", "tim", "doc", "dim"]`, for the most important parts of the index (e.g., norms, doc values, terms dictionaries).
* Using a vector-based preload setting, such as `index.store.preload: ["vec", "vex", "vem"]`, for search and aggregations.

> Note: It is essential to note that this setting can be dangerous if used on larger indices, as it would cause the file system cache to be trashed upon reopening after large merges. Therefore, it is crucial to carefully evaluate the trade-offs of using this setting in your specific use case.

### Avoid page cache thrashing by using modest readahead values for Linux
Here's a rephrased version of the text:

When using software raid with LVM or dm-crypt, Linux distributions often assign a high readahead value to the resulting block device (e.g., `/path.data` on an Elasticsearch instance). This can result in significant page cache thrashing, negatively impacting search and update performance. To resolve this issue, it's essential to identify the current readahead value.

To check the current readahead value for the block device, use `lsblk -o NAME,RA,MOUNTPOINT,TYPE,SIZE`. Consult your distribution's documentation on how to alter this value, as some may offer permanent settings (e.g., using a udev rule) or transient changes (via `blockdev --setra`). A recommended initial value for readahead is 128 KiB.

It's also essential to note that blockdev expects values in 512-byte sectors, whereas lsblk reports values in KiB. To temporarily set the readahead value to 128 KiB on `/dev/nvme0n1`, use the command `blockdev --setra 256 /dev/nvme0n1`.

### Using faster hardware 
* Also consider local storage over remote storage

### Searching for as few fields as possible
Using multiple fields in a query can significantly slow down search performance.
To improve speed, it's best to pre-aggregate values from these fields at index time and store them in a single field. 
This approach can be automated without altering the source documents using "copy_to"

#### Copy-to
The `copy-to` parameter allows copying values and joining them as a single field which provides much faster querying.
"
* It is the field value which is copied, not the terms (which result from the analysis process).
* The original _source field will not be modified to show the copied values.
* The same value can be copied to multiple fields, with "copy_to": [ "field_1", "field_2" ]
* You cannot copy recursively using intermediary fields.
"

### Pre-Index Data
Pre-indexing helps to make queries faster this is specially more useful such as in the case when you doing range queries and you have fixed list of ranges.


## References
* Chatgpt or LLM
* https://www.elastic.co/guide/en/elasticsearch/reference/current/tune-for-search-speed.html
* https://www.elastic.co/guide/en/elasticsearch/reference/current/preload-data-to-file-system-cache.html
* https://www.elastic.co/guide/en/elasticsearch/reference/current/copy-to.html