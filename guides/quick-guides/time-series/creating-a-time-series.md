1. Create and delete a time series.

Let's create a new time series.

```redis Create
TS.CREATE // Creates a new time series
    sensor1 // Keyname for the time series
    LABELS // Adds a label that can be either string or numeric values. Labels are key-value metadata we attach to data points, allowing us to group and filter. 
    area_id 32 // Label with area_id = 32
    region east // Label with region = east
    
```
We can get all the information and statistics about time series created.

```redis Read
TS.INFO sensor1 // Returns information and statistics on sensor1
 
TS.QUERYINDEX area_id=32 // Get all the time series matching with area_id=32

```

Let's delete a time series.

```redis Delete
EXPIRE sensor1 120 // Sets expiration of the time series to 120 seconds
 
DEL sensor1 // Deletes the entire time series
```

2. Manage data points

You can append new samples to your time series. Note that if the series does not exist, it will be automatically created.
```redis Add
TS.ADD // Adds a new data point
    sensor1 // Keyname for the time series
    1626434637914 // Timestamp
    26 // Value
 
TS.MADD // Adds multiple data points to a time series
    sensor1 // Keyname for the first time series
    1626435637914 // Timestamp
    28 // Value
    sensor1 // Keyname for the second time series, which can be a different one
    * // Timestamp
    35 // Value
 
TS.ADD 
    sensor1
    * // Will use  the current timestamp
    50
    
```

Let's get the last sample and the last sample matching the specific filter.

```redis Read
TS.GET sensor1 // Gets the last sample in the time series
 
TS.MGET FILTER area_id=32 // Returns the last sample from a time series with area_id=32

```

You can also update existing time series.

```redis Update
TS.ALTER // Updates existing data points
    sensor1 // Keyname for the timerseries
    1626434637914 // Timestamp
    26 // Value

```

To delete samples, specify the interval (inclusive) between two timestamps for a given time series.

```redis Delete
TS.DEL sensor1 1626434637914 1626434637964 // Deletes all the data points between two timestamps (inclusive)
 
TS.DEL sensor1 1626435637914 1626435637914 // To delete a single timestamp, use it as both the "from" and "to" timestamp

```
