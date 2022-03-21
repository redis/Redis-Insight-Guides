RedisTimeSeries is a time-series database (TSDB) module for Redis, by Redis.

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

### PRE-REQUISITES
**You will need:**

[Redis Stack Server](https://redis.io/download/#redis-downloads) >=6.2.0-v0 \
OR \
[RedisTimeSeries](https://oss.redis.com/redistimeseries/) >=1.0 \
OR \
A (free and ready to use) instance on [Redis Cloud](https://redis.com/try-free/?utm_source=redis\&utm_medium=app\&utm_campaign=redisinsight_doc_guide).
