### Basic CRUD operations

First, let's delete the movies graph (if exists). Note that the entire graph data is accessible using a single key.

```redis Delete Graph
DEL movies // deletes the movies graph
```

All Cypher queries in RedisGraph are executed via the `GRAPH.QUERY` command that takes two arguments
* the name of the graph
* the query

Let's add three nodes that represent actors and then add a node to represent a movie.
Note that the graph data structure 'movies' will be automatically created for us as and the nodes are added to it.

```redis Create Nodes
GRAPH.QUERY movies "CREATE (:Actor {name:'Mark Hamill', actor_id:1}), (:Actor {name:'Harrison Ford', actor_id:2}), (:Actor {name:'Carrie Fisher', actor_id:3})" // each node represents an actor.

GRAPH.QUERY movies "CREATE (:Movie {title:'Star Wars: Episode V - The Empire Strikes Back', release_year: 1980 , movie_id:1})" // this node represent a movie
```

Now we can create 'ACTED_IN' relationships between actors and the movies.

```redis Create Relationships
GRAPH.QUERY movies "MATCH (a:Actor),(m:Movie) WHERE a.actor_id = 1 AND m.movie_id = 1 CREATE (a)-[r:ACTED_IN {role:'Luke Skywalker'}]->(m) RETURN r" // Mark Hamill played Luke Skywalker in Star Wars: Episode V - The Empire Strikes Back'

// Let's add two more 'ACTED_IN' relationships
GRAPH.QUERY movies "MATCH (a:Actor), (m:Movie) WHERE a.actor_id = 2 AND m.movie_id = 1 CREATE (a)-[r:ACTED_IN {role:'Han Solo'}]->(m) RETURN r"  // Harrison Ford played Han Solo
GRAPH.QUERY movies "MATCH (a:Actor), (m:Movie) WHERE a.actor_id = 3 AND m.movie_id = 1 CREATE (a)-[r:ACTED_IN {role:'Princess Leila'}]->(m) RETURN r" // Carrie Fisher played Princess Leila
```

Let's get information about the movies.

```redis Pattern-matching Queries
GRAPH.QUERY movies "MATCH (m:Movie) RETURN m.title" // returns titles of all the movies

GRAPH.QUERY movies "MATCH (m:Movie) WHERE m.movie_id = 1 RETURN m" // returns information for the movie with ID 1.

GRAPH.QUERY movies "MATCH (a:Actor)-[r:ACTED_IN]-(m:Movie) WHERE m.movie_id = 1 RETURN a.name,m.title,r.role" // returns actors and roles they acted in the movie 'Star Wars: Episode V - The Empire Strikes Back' and what roles did they play?

```
   
You can count actors in our graph.

```redis Count Nodes
GRAPH.QUERY movies "MATCH (a:Actor) RETURN COUNT(a)"
```
And calculate the number of 'ACTED_IN' relationships in the graph.

```redis Count Relationships
GRAPH.QUERY movies "MATCH ()-[r:ACTED_IN]->() RETURN COUNT(r)"
```
Let's delete all 'ACTED_IN' relationships for Mark Hamill.

```redis Delete Relationships
GRAPH.QUERY movies "MATCH (a)-[r:ACTED_IN]->() WHERE a.actor_id = 1 DELETE r"    
```
You can also delete the movie "Star Wars: Episode V" from your graph. Note that when you delete a node, all of the node's incoming/outgoing relationships are also removed.
```redis Delete Nodes
GRAPH.QUERY movies "MATCH (m) WHERE m.movie_id = 1 DELETE m"
```

### Properties

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
