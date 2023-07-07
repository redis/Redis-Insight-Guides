Functions can react to events within Redis with key space triggers. Most of the events are triggered by command invocation but includes also events for when a key is expired or evicted from the database.

For the full list of supported events, please refer to the [Redis Key Space notifications page](https://redis.io/docs/manual/keyspace-notifications/#events-generated-by-different-commands).

The following code creates a new key space trigger that adds a new field to a hash with the latest update time. 

```JavaScript

```

Load the code in your database:

```redis Load keyspace example
TFUNCTION LOAD REPLACE “ADD CODE”
```

Add a new hash to.

```redis Set an example
HSET example:1 name “John Doe”
```

Check if the last updated time is added to the example.

```redis View result
HGETALL example:1
```
