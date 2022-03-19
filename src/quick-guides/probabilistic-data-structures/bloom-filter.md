Click on the button, see the command and the comments in the Workbench editor, and then run it.

Bloom filters are used to determine, with a high degree of certainty, whether an element is a member of a set. They typically exhibit better performance and scalability when inserting items.

```redis Create Filter
BF.RESERVE bloom 0.001 100 // create a Bloom filter as “bloom” key with desired false positive rate of 0.1% (0.001) and 100 entries intended to be added to the filter

```
```redis Add Items
BF.ADD bloom foo // adds the item “foo” to the bloom filter
BF.MADD bloom bar baz // adds the items "bar" and "baz" to the bloom filter

```
```redis Check If The Item May Exist
BF.EXISTS bloom foo // “1” if the item may exist
BF.EXISTS bloom foo1 // “0” if the item certainly does not exist
BF.MEXISTS bloom bar baz // determines if one or more items exist in the filter or not

```
```redis Information About The Filter
BF.INFO bloom // returns information about the key

```
```redis Create And Add Items
BF.INSERT bloomFilter ITEMS foo bar baz // Adds three items to the filter named "bloom" with default parameters, if the filter does not already exist
BF.INSERT bloomF NOCREATE ITEMS foo bar // Adds 2 items to a filter with an error if the filter does not already exist
BF.INSERT newBloomFilter CAPACITY 10000 ITEMS hello // Adds one item to a filter with a capacity of 10000 if the filter does not already exist

```
