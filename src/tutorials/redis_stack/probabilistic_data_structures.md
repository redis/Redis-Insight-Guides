Probabilistic data structures (PDS) are a class of data struture that trade off precision for efficiences of speed and storage space.  Probabilistic datas structures allow us to get approximate results faster and by using significantly less memory than with equivalent deterministic data structures.

Redis Stack supports 4 of the most well-known PDS:

- Bloom filters
- Cuckoo filters
- Count-Min Sketch
- Top-K

In the rest of this tutorial we'll show you how to use a Bloom filter to save expensive calls to your main database, while also saving a lot of memory when compared to using sets or hashes.

A Bloom filter is a probabilistic data structure that enables you to check if an element is present in a set using a small fixed size memory space. **It can guarantee the absence of an element from a set, but it can only give an estimation about its presence**. So when it responds that an element is not present in a set (a negative answer), you can be sure that indeed is the case. However, one out of every N positive answers will be wrong.  This means that you'll occasionally get a false positive result.

Even though it looks unusual at a first glance, this kind of uncertainty still has an important place in computer science. There are many cases out there where a negative answer will prevent very costly operations, and for which using a deterministic data structure such as a set would not be possible due to the storage requirements associated with it.

How can a Bloom filter be useful to our bike shop? For starters, we could keep a Bloom filter that stores all usernames of people who've already registered with our service. That way, when someone is creating a new account we can very quickly check if that username is free. If the answer is yes, we'd still have to go and check the main database for the precise result, but if the answer is no, we can skip that call and ask the user to choose a different username without consulting the main database at all. 

Another, perhaps more interesting example is for showing better and more relevant ads to users. We could keep a Bloom filter per user with all the products they've bought from the shop, and when we get a list of products from our suggestion engine we could check it against this filter.


```redis Add all bought product ids in the Bloom filter
BF.MADD user:778:bought_products 4545667 9026875 3178945 4848754 1242449
```

Just before we try to show an ad to a user, we can first check if that product id is already in their "bought products" Bloom filter. If the answer is yes - we might choose to check the main database, or we might skip to the next recommendation from our list. But if the answer is no, then we know for sure that our user hasn't bought that product:

```redis Has a user bought this product?
BF.EXISTS user:778:bought_products 1234567  // No, the user has not bought this product
BF.EXISTS user:778:bought_products 3178945  // The user might have bought this product
```
