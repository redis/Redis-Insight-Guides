RedisGraph is a Redis module that enables creating and querying **property graphs**.

Common examples of graph use cases:
* Supply chains: recalculate an alternative route from factory A to consumer B to avoid road closures, understand the spread of the product (in which countries/cities is it used), etc.
* Social networks: discover who the friends of my friends are, personalized content recommendations, etc.
* Geo navigation: list all shops in an area of 1km, with an average rating higher than 4.2, etc.
* Fraud detection: understand whether a call/sms follows a regular calling pattern, investigate whether a transaction between a sender and a receipient is valid, etc.
* Identity and access management: define complex resources access permissions as a graph and enable rapid real-time verification of these permissions with a single query.

RedisGraph implements a unique data storage and processing solution that translates [Cypher](https://oss.redis.com/redisgraph/cypher_support/) queries to matrix operations executed over a GraphBLAS engine to deliver the fastest and most efficient way to store, manage, and process connected data in graphs.

With RedisGraph, you can process complex transactions 10 - 600 times faster than with traditional graph solutions while using 50 - 60% less memory resources than other graph databases!


### PRE-REQUISITES
**You will need:**

[Redis Stack Server](https://redis.io/download) >=6.2.0-v0 \
OR \
[RedisGraph](https://oss.redis.com/redisgraph/) >=2.0 \
OR \
A free Redis Stack instance on [Redis Cloud](https://redis.com/try-free/?utm_source=redis\&utm_medium=app\&utm_campaign=redisinsight_doc_guide).
