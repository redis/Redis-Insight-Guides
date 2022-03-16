RedisTimeSeries is a time-series database (TSDB) module for Redis, by Redis.

RedisTimeSeries can hold multiple time series.

Each **time series** is accessible via key (like any other Redis data structure) and comprises:
* Raw samples: each sample comprises a {time tag, value} pair. 
  * Time tags are measured in milliseconds since January 1st, 1970, at 00:00:00.
  * Time tags can be specified by the client or filled automatically by the server.
  * 64-bit floating-point values.
\
The intervals between time tags can be constant or variable.
\
Raw samples can be reported in-order or out-of-order.
\
Duplication policy for samples with identical time tags: block/first/last/min/max/sum.
* A configurable retention period.
\
Raw samples older than the retention period (relative to the raw sample with the highest time tag) are discarded.
* Series Metadata: a set of name-value pairs (e.g., room = 3; sensorType = ‘xyz’).
\
RedisTimeSeries supports cross-time-series commands. One can, for example, aggregate data over all sensors in the same room or all sensors of the same type.
* Zero or more compactions: 
\
Compactions are an economical way to retain historical data.
\
Each compaction is defined by:
* A timeframe. E.g., 10 minutes
* An aggregator: min, max, sum, avg, …
* A retention period. E.g., 10 year
\
For example, the following compaction: {10 minutes; avg; 10 years} will store the average of the raw values measured in each 10-minutes time frame - for 10 years.

Common examples of time series data:
* Sensor data: e.g., temperatures or fan velocity for a server in a server farm
* Stock prices
* Number of vehicles passing through a given road (counts per 1-minute timeframes)

Common examples for applications that use time-series databases:
* IoT & Sensor Monitoring: monitor and react to a stream of events emitted by devices
* Application Performance/Health Monitoring: monitor the performance and availability of applications and services.
* Real-time Analytics: process, analyze and react in real-time (e.g., for selling equities, performing predictive maintenance, product recommendations, or price adjustments)

You can ingest and query millions of samples and events at the speed of Redis.

The entire time series is held in memory (and optionally persisted to disk).

Each time series is stored in a single Redis shard (+ optional shards as replicas).

A shard may contain multiple time series.


**Pre-requisites**


Follow these instructions to set up the RedisTimeSeries module on Redis OSS.

For working with Hashes you will need Redis >=6, [RediSearch](https://oss.redis.com/redisearch/) >=2.0.

For working with JSON you will need Redis >=6, [RediSearch](https://oss.redis.com/redisearch/) >=2.2 and [RedisJSON](https://oss.redis.com/redisjson/) >=2.0.

You could also create a free and ready to use instance on [Redis Cloud](https://redis.com/try-free/?utm_source=redis\&utm_medium=app\&utm_campaign=redisinsight_doc_guide).



Follow [these](https://oss.redis.com/redistimeseries/) instructions to set up the RedisTimeSeries module on OSS or install [Redis Stack](TBD).

You could also create a free and ready to use instance on [Redis Cloud](https://redis.com/try-free/?utm_source=redis\&utm_medium=app\&utm_campaign=redisinsight_doc_guide).
