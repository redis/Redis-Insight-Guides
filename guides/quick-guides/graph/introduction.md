RedisGraph is a queryable [Property Graph](https://github.com/opencypher/openCypher/blob/master/docs/property-graph-model.adoc) data structure for Redis.

Common graph use cases:
* Identity and access management: define complex resources access permissions as a graph and enable rapid real-time verification of these permissions with a single query.
* Supply chains: recalculate an alternative route from factory A to consumer B to avoid road closures, understand the spread of the product (in which countries/cities is it used), etc.
* Social networks: discover who the friends of my friends are, personalized content recommendations, etc.
* Geo navigation: list all shops within a 1km radius of a given location, with an average rating higher than 4.2, etc.
* Fraud detection: understand whether a call/sms follows a regular calling pattern, investigate whether a transaction between a sender and a receipient is valid, etc.

 Redisgraph uses [sparse matrices](https://en.wikipedia.org/wiki/Sparse_matrix) to represent the [adjacency matrix](https://en.wikipedia.org/wiki/Adjacency_matrix). RedisGraph translates [Cypher](https://oss.redis.com/redisgraph/cypher_support/) queries to matrix operations executed over a GraphBLAS engine to deliver the fastest and most efficient way to store, manage, and process connected data in graphs.

### PRE-REQUISITES
**You will need:**

[Redis Stack Server](https://redis.io/download) >=6.2.0-v0 \
OR \
[RedisGraph](https://oss.redis.com/redisgraph/) >=2.0 \
OR \
A free Redis Stack instance on [Redis Cloud](https://redis.com/try-free/?utm_source=redis\&utm_medium=app\&utm_campaign=redisinsight_graph_guide).
