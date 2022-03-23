You can filter by value, timestamp and by labels.
 
```redis Filtering by label
TS.MRANGE - + FILTER area_id=32 // Shows data from all time series that have a label of area_id with a value of 32. The results will be grouped by time series.

```

Let's see only the information that fits into a specific range of values.

```redis Filtering by value
TS.RANGE sensor1 - + FILTER_BY_VALUE 25 30 // Returns all data points whose value sits between 25 and 30, inclusive.
 
TS.MRANGE - +  FILTER_BY_VALUE 20 30 FILTER region=east // Filters on multiple series
```

We also can filter the data based on timestamps.

```redis Filtering by timestamp
TS.RANGE sensor1 - + FILTER_BY_TS 1626434637914 1626435637914 // Retrieves the data points for specific timestamps on one or multiple time series
 
TS.MRANGE - +  FILTER_BY_TS 1626435230501 1626443276598 FILTER region=east // Filters on multiple time series 
```
