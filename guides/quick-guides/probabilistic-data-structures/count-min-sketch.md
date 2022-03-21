A count-min sketch is generally used to determine the frequency of events in a stream. You can query the count-min sketch get an estimate of the frequency of any given event.

You can initialize the count-min sketch to custom dimensions and specify width (number of counters in each array) and depth (number of counter-arrays).

```redis Initialize A Sketch With Dimensions
CMS.INITBYDIM CMS 2000 5 // initializes the count-min sketch with 2000 counters in each array and 5 counter-arrays

```

To initialize the count-min sketch to accomodate requrested tolerances, you need to specify the estimated size of error (affects the width of the sketch) and the desired probability for inflated count.


```redis Initialize A Sketch With Tolerances
CMS.INITBYPROB CMSp 0.001 0.01 // initializes the Count-Min Sketch to accommodate requested capacity with 0.1% estimation of error and 1% desired probability for inflated count

```

You can also increase the count of items by increment

```redis Update
CMS.INCRBY CMS foo 10 bar 42 // increases the count of “foo” item by 10 and “bar” by 42

```

Let's return the count in a sketch

```redis Return Query
CMS.QUERY CMS foo bar // returns the count of “foo” and “bar”
```

You can find more information about width, depth and total count of your sketch.

```redis Information About The Sketch
CMS.INFO CMS

```