**In Redis, you can model documents using:**
*   [Hashes](https://redis.io/topics/data-types#hashes)
*   [JSON](https://oss.redis.com/redisjson/)

**Hashes:**
*   Best for representing rows in a relational database table
*   Schemaless collections of flat field-value pairs
*   Support CRUD operation on field-value pairs at any time, not just initial declaration

**JSON:**
*   Best for representing documents in a document store
*   Full support of the JSON format with nested objects and nested arrays
*   Supports [JSONPath](http://goessner.net/articles/JsonPath/) -like syntax for selecting elements inside
*   Supports atomic CRUD operations

***
### PRE-REQUISITES
**You will need:**

[Redis Stack Server](https://redis.io/download/#redis-downloads) >=6.2.0-v0 \
OR \
[RediSearch](https://oss.redis.com/redisearch/) >=2.0 and [RedisJSON](https://oss.redis.com/redisjson/) >=2.0 \
OR \
A (free and ready to use) instance on [Redis Cloud](https://redis.com/try-free/?utm_source=redis\&utm_medium=app\&utm_campaign=redisinsight_doc_guide).