1. Basic CRUD operations

First, let's delete the movies graph (if exists). Note that the entire graph data is accessible using a single key.
```redis Delete Graph

    DEL movies // deletes the movies graph

```
Let's add three graph nodes to represent actors and then add a node to represent a movie.
Note that the movies graph will be automatically created as well.
 
```redis Create Nodes
  
    GRAPH.QUERY movies "CREATE (:Actor {name:'Mark Hamill', actor_id:1}), (:Actor {name:'Harrison Ford', actor_id:2}), (:Actor {name:'Carrie Fisher', actor_id:3})" // each node represents an actor.

    GRAPH.QUERY movies "CREATE (:Movie {title:'Star Wars: Episode V - The Empire Strikes Back', release_year: 1980 , movie_id:1})" // this node represent a movie

```

Now we can 'played in' relationships for actors and the movie.

```redis Create Relationships
    GRAPH.QUERY movies "MATCH (a:Actor),(m:Movie) WHERE a.actor_id = 1 AND m.movie_id = 1 CREATE (a)-[r:Acted_in {role:'Luke Skywalker'}]->(m) RETURN r" // Mark Hamill played Luke Skywalker in Star Wars: Episode V - The Empire Strikes Back'

    // Let's add two more 'played in' relationships
    GRAPH.QUERY movies "MATCH (a:Actor), (m:Movie) WHERE a.actor_id = 2 AND m.movie_id = 1 CREATE (a)-[r:Acted_in {role:'Han Solo'}]->(m) RETURN r"  // Harrison Ford played Han Solo
    GRAPH.QUERY movies "MATCH (a:Actor), (m:Movie) WHERE a.actor_id = 3 AND m.movie_id = 1 CREATE (a)-[r:Acted_in {role:'Princess Leila'}]->(m) RETURN r" // Carrie Fisher played Princess Leila

```

Let's get information about the movie.

```redis Pattern-matching Queries
    GRAPH.QUERY movies "MATCH (m:Movie) RETURN m.title" // returns titles of all the movies

    GRAPH.QUERY movies "MATCH (m:Movie) WHERE m.movie_id = 1 RETURN m" // returns information for the movie with ID 1.

    GRAPH.QUERY movies "MATCH (a:Actor)-[r:Acted_in]-(m:Movie) WHERE m.movie_id = 1 RETURN a.name,m.title,r.role" // returns actors and roles they played in the movie 'Star Wars: Episode V - The Empire Strikes Back' and what roles did they play?

```
   
You can count actors in our graph.

```redis Count Nodes
    GRAPH.QUERY movies "MATCH (a:Actor) RETURN COUNT(a)"

```
And calculate the number of 'played in' relationships in the graph.

```redis Count Relationships
    GRAPH.QUERY movies "MATCH ()-[r:Acted_in]->() RETURN COUNT(r)"

```
Let's delete all 'played in' relationships for Mark Hamill.

```redis Delete Relationships
    GRAPH.QUERY movies "MATCH (a)-[r:Acted_in]->() WHERE a.actor_id = 1 DELETE r"
    
```
You can also delete the "Star Wars: Episode V" movie from your graph. Note that when you delete a node, all of the node's incoming/outgoing relationships are also removed.
```redis Delete Nodes

    GRAPH.QUERY movies "MATCH (m) WHERE m.movie_id = 1 DELETE m"

```

2. Properties

Let's create a node with properties of multiple types.
```redis Property types
    GRAPH.QUERY movies "CREATE (:node {a:3+4, b:3/4, c:3=4, d: '3 4', e:[3+4, 3/4, 3=4, '3 4']})" // a: a 64-bit signed integer, b: a 64-bit double, c: a string literal, d: a Boolean value, e: an array of primitive types

```
Now we can get an array of all the node's property names.
```redis Get node's property names
    GRAPH.QUERY movies "MATCH (n:node) return keys(n)"

```

And get an array of all property names of all nodes in the graph.
```redis Get all property names
    GRAPH.QUERY movies "CALL db.propertyKeys() YIELD propertyKey"

```

3. Indexing 

Let's get all the movies where Mark Hamill acted.
```redis Textual match
    GRAPH.QUERY movies "MATCH (a:Actor)-[r:Acted_in]-(m:Movie) WHERE a.name = 'Mark Hamill' RETURN a.name,m.title"

```
We also can inspect the execution plan to better understand how the query will get executed.
To do so, we need to construct a query execution plan.
```redis Inspect execution plan
    GRAPH.EXPLAIN movies "MATCH (a:Actor)-[r:Acted_in]-(m:Movie) WHERE a.name = 'Mark Hamill' RETURN a.name,m.title" // Constructs a query execution plan but does not run it.

```
Let's create an index on the 'name' property of all nodes with label 'Actor'.

```redis Create index
    GRAPH.QUERY movies "CREATE INDEX FOR (a:Actor) ON (a.name)"

```
So we can inspect the execution plan after creating the index.
```redis Inspect updated execution plan
    GRAPH.EXPLAIN movies "MATCH (a:Actor)-[r:Acted_in]-(m:Movie) WHERE a.name = 'Mark Hamill' RETURN a.name,m.title"

```
