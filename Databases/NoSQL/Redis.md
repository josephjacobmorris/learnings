# Redis

## Introduction

Redis is a in-memory key-value store NoSql database. It is primarily used for caching.It supports strings, hashes,List,sets and sorted sets.

## Top Redis Use-cases

### Caching

### Session Store

Session ids are stored in redis and hence making the servers stateless. Since redis is in-memory once it crashes the data is lost to avoid we can persist the session info to disk which is time consuming so in production  replication is preferred.

### Distributed Lock

Used to get lock on shared redources. If available it returns 1 else 0 when the  setnx is used.

<img src="distributed_lock.png">

But the above implementation is not with issues so we can use exising implementation library

<img src="redis_distributed_lock_impls.png">

### Rate Limiters

Limit the number of requests per given interval using counter with TTL value. It can also be used to implement rate limiting algorithms such as the leaky bucket.

### Rank Leaderboards

Using sorted sets.Allows for retrieval in log time.

## References

* <https://www.youtube.com/watch?v=a4yX7RUgTxI&ab_channel=ByteByteGo>
