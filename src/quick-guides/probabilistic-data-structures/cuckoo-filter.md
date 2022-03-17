Cuckoo filters are used to determine, with a high degree of certainty, whether an element is a member of a set. They are quick on check operations and also allow deletions.
    
```redis Create Filter
CF.RESERVE cuckoo 100 10 2// create a Cuckoo Filter as “cuckoo” key with a single sub-filter for the initial amount of 100 for items, 10 items in each bucket and 2 attempts to swap items between buckets before declaring filter as full and creating an additional filter

```
```redis Add Items
CF.ADD cuckoo foo // adds the “foo” item to the cuckoo filter
CF.ADD cuckoo bar // adds the “bar” item to the cuckoo filter
CF.ADD cuckoo baz // adds the “baz” item to the cuckoo filter
CF.ADD cuckoo baz // adds one more “baz” item to the cuckoo filter

```
```redis Check If The Item May Exist
CF.EXISTS cuckoo foo // “1” if the item may exist
CF.EXISTS cuckoo foo1 // “0” if the item certainly does not exist

```
```redis Delete An Item
CF.DEL cuckoo bar // delete the “bar” item once from the filter
CF.DEL cuckoo baz // if the item was added multiple times, it will still be present

```
```redis Count Items
CF.COUNT cuckoo baz // returns the number of times the “baz” may be in the filter


```
```redis Information About The Filter
CF.INFO cuckoo // returns information about the key

```