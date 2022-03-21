Bloom filters are used to determine, with a high degree of certainty, whether an element is a member of a set. They typically exhibit better performance and scalability when inserting items.

Let's create a bloom filter with desired probability for false positives.
Also we can add capacity - the number of entries intended to be added to the filter. If your filter allows scaling, performance will begin to degrade after adding more items than this number.

```redis Create Filter
BF.RESERVE bloom 0.001 100 // new bloom filter as the “bloom” key with desired false positive rate of 0.1% (0.001) and 100 entries intended to be added to the filter

```

So let's add items to our new bloom filter

```redis Add Items
BF.ADD bloom foo // adds the “foo” item
BF.MADD bloom bar baz // adds "bar" and "baz" items

```
Now we can determine whether an item may exist in the bloom filter or not.

```redis Check If May Exist
BF.EXISTS bloom foo // “1” if the item may exist
BF.EXISTS bloom foo1 // “0” if the item certainly does not exist
BF.MEXISTS bloom bar baz // determines if one or more items exist in the filter or not

```
You can find more information about your bloom filter

```redis Information About The Filter
BF.INFO bloom // returns information about the "bloom" filter

```
Also you can create a new filter if it does not exist and insert items to it

```redis Create And Add Items
BF.INSERT bloomFilter ITEMS foo bar baz // creates the "bloomFilter" key and adds three items to it, if the filter does not already exist
BF.INSERT newBloomFilter CAPACITY 10000 ITEMS hello // creates the "newBloomFilter" key and adds three items to it, if the filter does not already exist
BF.INSERT bloomF NOCREATE ITEMS foo bar // tries to add 2 items to a filter with an error if the filter does not already exist


```
