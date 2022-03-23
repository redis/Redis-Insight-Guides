### Indexing 
Note: this section of the guide uses commands that only work with RedisGraph 2.8 and above.

Let's query all the movies in which Mark Hamill was an actor.

```redis MATCH by property name
GRAPH.QUERY movies "MATCH (a:Actor)-[r:ACTED_IN]-(m:Movie) WHERE a.name = 'Mark Hamill' RETURN a.name,m.title"
```

RedisGraph allows you to inspect the execution plan to better understand how the query gets executed.
To do so, we can use the `GRAPH.EXPLAIN` command in the same way as `GRAPH.QUERY`.

```redis Inspect execution plan
GRAPH.EXPLAIN movies "MATCH (a:Actor)-[r:ACTED_IN]-(m:Movie) WHERE a.name = 'Mark Hamill' RETURN a.name,m.title" // Constructs a query execution plan but does not run it.

```
The last line of the query plan does a `Node By Label Scan`.  This means that in a large graph we will scan through all the nodes in order to find 'Mark Hamill'.  We can optimise this by creating an index.

Let's create an index on the 'name' property of all nodes with label 'Actor'.

```redis Create index
GRAPH.QUERY movies "CREATE INDEX FOR (a:Actor) ON (a.name)"
```
So we can inspect the execution plan after creating the index.

```redis Inspect updated execution plan
GRAPH.EXPLAIN movies "MATCH (a:Actor)-[r:ACTED_IN]-(m:Movie) WHERE a.name = 'Mark Hamill' RETURN a.name,m.title"
```
The last line of the query plan does a `Node By Index Scan`.

Alternively, you can also profile the query with `GRAPH.PROFILE`.  Note that in this case it will run `GRAPH.EXPLAIN` and `GRAPH.QUERY` in one go.

```redis Profile your query
GRAPH.PROFILE movies "MATCH (a:Actor)-[r:ACTED_IN]-(m:Movie) WHERE a.name = 'Mark Hamill' RETURN a.name,m.title"
```

### Full-text Search indices

In addition to secondary indexing, you can also create full-text search indices. This is a great feature for "graph-aided search" use cases, such as your LinkedIn search.

Let's create a full-text index for the 'name' property of all nodes with label 'Actor'.

```redis Create a full-text search index
GRAPH.QUERY movies "CALL db.idx.fulltext.createNodeIndex('Actor', 'name')"
```

We will now use this index to match actors that their name contains a given word.

```redis MATCH by fuzzy property
GRAPH.QUERY movies "CALL db.idx.fulltext.queryNodes('Actor', '%Mork%') YIELD node RETURN node.name"
```
The avoe query uses a [Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance) of 1 to find Mark back.

RedisGraph supports the [query syntax of RediSearch](https://oss.redis.com/redisearch/Query_Syntax/).