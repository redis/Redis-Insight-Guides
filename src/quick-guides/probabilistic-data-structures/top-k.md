A top-k maintains a list of k most frequently seen items.
    
```redis Initialize a TopK
TOPK.RESERVE TopK 50 2000 7 0.925 // initializes the “TopK” with 50 counters kept in each array, 2000 arrays and 0.925 probability of reducing a counter in an occupied bucket

```
```redis Add Items
TOPK.ADD TopK foo bar 42 // adds “foo”, “bar” and “42” items to the TopK

```
```redis Increase The Score
TOPK.INCRBY TopK foo 3 bar 2 42 30 // increases the score of “foo” by 3, “bar” by 2 and “42” by 30. Returns “nil” if no change to Top-K list occurred, else returns item dropped from the list


```
```redis Check If The Item Is In Top-K items
TOPK.QUERY TopK 42 nonexist // for each item requested, return 1 if item is in Top-K, otherwise 0.

```
```redis Return The Full List Of Items in Top-K list
TOPK.LIST TopK // return full list of items in Top K list

```
```redis Information About The Filter
TOPK.INFO TopK

```