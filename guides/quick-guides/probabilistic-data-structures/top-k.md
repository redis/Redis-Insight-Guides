A top-k maintains a list of k most frequently seen items.

Let's initialize a TopK with width (a number of counters kept in each array), depth (number of arrays) and decay (probability of reducing a counter in an occupied bucket).

```redis Initialize a TopK
TOPK.RESERVE TopK 50 2000 7 0.925 // initializes the “TopK” with 50 counters kept in each array, 2000 arrays and 0.925 probability of reducing a counter in an occupied bucket

```

Let's add items to the TopK filter.

Note that if an item enters the Top-K list, the item which is expelled is returned.the cuckoo filter.

```redis Add Items
TOPK.ADD TopK foo bar 42 // adds “foo”, “bar” and “42” items to the TopK.

```

You can increase the score of an item in the TopK by increment.

Returns “nil” if no change to Top-K list occurred, else returns item dropped from the list.

```redis Increase The Score
TOPK.INCRBY TopK foo 3 bar 2 42 30 // increases the score of “foo” by 3, “bar” by 2 and “42” by 30.

```
Now we can check whether an item is one of Top-K items or not.

```redis Check If In Top-K items
TOPK.QUERY TopK 42 nonexist // for each item requested, returns 1 if item is in Top-K, otherwise 0.

```

Let's see the full list of items in our Top K list.

```redis Return The Full List
TOPK.LIST TopK

```
You can see the number of required items (k), width, depth and decay values in your TopK filter.

```redis Information About The Filter
TOPK.INFO TopK

```