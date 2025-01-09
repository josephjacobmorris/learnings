# Splunk

## Introduction
Splunk Observability is a suite of tools designed to provide real-time visibility, monitoring, and troubleshooting for modern IT environments. It offers comprehensive solutions for infrastructure monitoring, application performance management (APM), and log management. By integrating metrics, traces, and logs, Splunk Observability helps teams quickly identify and resolve performance issues, optimize resource usage, and ensure the reliability of distributed systems. Its cloud-native architecture supports scalability and seamless integration with various cloud platforms, making it an ideal choice for organizations looking to enhance their observability capabilities.

## Splunk Enterprise Components
### Processing
* Indexer
* Forwarder
* Search Head - Web UI + Search is triggered here

### Management
* Cluster manager
* License Server
* Deployment Server

## Splunk Data Pipeline
* Input Data (logs etc.)
* Parsing (understanding, extracting fields etc)
* Indexing (for faster searching and querying)
* Searching (Where the actual data is used to build reports etc.)

## Search Query
### Basics
* `*` will work as wildcard
* search terms are not case sensitive
* `AND`,`OR` ,`NOT` boolean operations are supported
* Order of evaluation of boolean operations is NOT, OR , AND
* If no operation is specified `AND` is implied.
* exact phrases can be searched using double quotes and use \ to espace " in search terms 
### Validity
* Search Job - needs to run every 10 mins 
* Shared Job executions - will store results for 7 days
### Performance
* Search for specific interval
* use index,sourcetype and host
* inclusivity over exclusion
* use in ,or over wildcard
* reduce the results in the first steps for best performance

## Splunk Access Level
### Splunk Enterprise
* User
* Power User
* Admin

### Splunk Cloud
* sc_admin
* power
* user
## Syntax

### `iplocation`
### `geostats`
### `stats`
### `rename`
### `fields`
### `erex`
### `rex`
### `eval`


## References
* Udemy : The Complete Splunk Beginner Course
* https://community.splunk.com/
* https://docs.splunk.com/Documentation
* 