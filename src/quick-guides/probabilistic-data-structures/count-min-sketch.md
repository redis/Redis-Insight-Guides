A count-min sketch is generally used to determine the frequency of events in a stream. You can query the count-min sketch get an estimate of the frequency of any given event.

You can initialize the Count-Min Sketch to custom dimensions.

```redis Initialize A Sketch
CMS.INITBYDIM CMS 2000 5 // initializes the Count-Min Sketch with 2000 counters in each array and 5 counter-arrays
CMS.INITBYPROB CMSp 0.001 0.01 // initializes the Count-Min Sketch to accommodate requested capacity with 0.1% estimation of error and 1% desired probability for inflated count


```
```redis Update
CMS.INCRBY CMS foo 10 bar 42 // increases the count of “foo” item by 10 and “bar” by 42

```
```redis Return Query
CMS.QUERY CMS foo bar // returns count of “foo” and “bar”


```
```redis Information About The Sketch
CMS.INFO CMS // returns information about the sketch