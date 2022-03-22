### Indexing 

Let's query all the movies in which Mark Hamill was an actor.

```redis Textual match
GRAPH.QUERY movies "MATCH (a:Actor)-[r:ACTED_IN]-(m:Movie) WHERE a.name = 'Mark Hamill' RETURN a.name,m.title"
```

RedisGraph allows you to inspect the execution plan to better understand how the query gets executed.
To do so, we can use the `GRAPH.EXPLAIN` command in the same way as `GRAPH.QUERY`.

```redis Inspect execution plan
GRAPH.EXPLAIN movies "MATCH (a:Actor)-[r:ACTED_IN]-(m:Movie) WHERE a.name = 'Mark Hamill' RETURN a.name,m.title" // Constructs a query execution plan but does not run it.

```
The last line of the query plan does a 'Node By Label Scan'.  This means that in a large graph we will scan through all the nodes in order to find 'Mark Hamill'.  We can optimise this by creating an index.

Let's create an index on the 'name' property of all nodes with label 'Actor'.

```redis Create index
GRAPH.QUERY movies "CREATE INDEX FOR (a:Actor) ON (a.name)"
```
So we can inspect the execution plan after creating the index.

```redis Inspect updated execution plan
GRAPH.EXPLAIN movies "MATCH (a:Actor)-[r:ACTED_IN]-(m:Movie) WHERE a.name = 'Mark Hamill' RETURN a.name,m.title"
```

Alternively, you can also profile the query with `GRAPH.PROFILE`.  Note that in this case it will run `GRAPH.EXPLAIN` and `GRAPH.QUERY` in one go.

```redis Profile your query
GRAPH.PROFILE movies "MATCH (a:Actor)-[r:ACTED_IN]-(m:Movie) WHERE a.name = 'Mark Hamill' RETURN a.name,m.title"
```

You can also create a full-text indices. Let's create a full-text index for the 'name' property of all nodes with label 'Actor'

```redis Construct a full-text index
GRAPH.QUERY movies "CALL db.idx.fulltext.createNodeIndex('Actor', 'name')"
```

We will now use this index to match actors that their name contains a given word.

```redis Match contained words using full-text index
GRAPH.QUERY movies "CALL db.idx.fulltext.queryNodes('Actor', 'Mark') YIELD node RETURN node.name"
```
