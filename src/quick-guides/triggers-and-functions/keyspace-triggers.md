Functions can react to events within Redis with key space triggers. Most of the events are triggered by command invocation but includes also events for when a key is expired or evicted from the database.

For the full list of supported events, please refer to the [Redis Key Space notifications page](https://redis.io/docs/manual/keyspace-notifications/#events-generated-by-different-commands).

The following code creates a new key space trigger that adds a new field to a hash with the latest update time. 

Load the code in your database:

```redis Load keyspace example
TFUNCTION LOAD REPLACE "#!js name=myFirstLibrary api_version=1.0\n 
    function addLastUpdatedField(client, data) {
        if(data.event == 'hset') {
            var currentDateTime = Date.now();
            client.call('hset', data.key, 'last_updated', currentDateTime.toString());
        }
    } 

    redis.registerKeySpaceTrigger('addLastUpdated', 'fellowship:', addLastUpdatedField);" // Register the KeySpaceTrigger 'AddLastUpdated' for keys with the prefix 'fellowship' with a callback to the function 'addLastUpdatedField'
```

Add a new hash to.

```redis Set an example
HSET fellowship:1 
    name "Frodo Baggins" 
    title "The One Ring Bearer"
```

Check if the last updated time is added to the example.

```redis View result
HGETALL fellowship:1
```
