**Triggers and Functions** is a JavaScript engine for data processing in Redis. This engine enables both on-demand and event-driven processing of Redis data.

To use Triggers and Functions, you need to write functions that describe how the data should be processed. Then, you register triggers that define the keyspace notifications or streams that will execute these functions.

Common examples of **Triggers and Functions**:

* Automatic Expire.
* Enrich and transform data.
* Batch operations

### PRE-REQUISITES
**You will need:**

[Redis Stack Server](https://redis.io/download/?utm_source=redis\&utm_medium=app\&utm_campaign=redisinsight_triggers_and_functions_guide) >= 7.2.0-RC2 \
OR \
[Triggers and Functions](https://redis.io/docs/interact/programmability/triggers-and-functions) >=2.0 \
OR \
A free Redis Stack instance on [Redis Cloud](https://redis.com/try-free/?utm_source=redis\&utm_medium=app\&utm_campaign=redisinsight_triggers_and_functions_guide).
