Click on the button, see the command and the comments in the Workbench editor, and then run it.

1. Basic CRUD operations

    ```redis Delete Graph
    // First, let's delete the movies graph (if exists).
    // Note that the entire graph data is accessible using a single key
    DEL movies

    ```
    ```redis Create Nodes
    // Let's add three graph nodes. Each node represents an actor.
    // Note that the movies graph will be automatically created as well
    GRAPH.QUERY movies "CREATE (:Actor {name:'Mark Hamill', actor_id:1}), (:Actor {name:'Harrison Ford', actor_id:2}), (:Actor {name:'Carrie Fisher', actor_id:3})"

    // Let's add a node that represents a movie
    GRAPH.QUERY movies "CREATE (:Movie {title:'Star Wars: Episode V - The Empire Strikes Back', release_year: 1980 , movie_id:1})"

    ```
    ```redis Create Relationships
    // Let's create a 'played in' relationship:
    // Mark Hamill played in Star Wars: Episode V
    GRAPH.QUERY movies "MATCH (a:Actor),(m:Movie) WHERE a.actor_id = 1 AND m.movie_id = 1 CREATE (a)-[r:Acted_in {role:'Luke Skywalker'}]->(m) RETURN r"

    // Let's add two more 'played in' relationships
    GRAPH.QUERY movies "MATCH (a:Actor), (m:Movie) WHERE a.actor_id = 2 AND m.movie_id = 1 CREATE (a)-[r:Acted_in {role:'Han Solo'}]->(m) RETURN r"
    GRAPH.QUERY movies "MATCH (a:Actor), (m:Movie) WHERE a.actor_id = 3 AND m.movie_id = 1 CREATE (a)-[r:Acted_in {role:'Princess Leila'}]->(m) RETURN r"

    ```
    ```redis Pattern-matching Queries
    // What are the titles of all the movies?
    GRAPH.QUERY movies "MATCH (m:Movie) RETURN m.title"

    // What is the information for the movie with the ID of 1?
    GRAPH.QUERY movies "MATCH (m:Movie) WHERE m.movie_id = 1 RETURN m"

    // Who are the actors in the movie 'Star Wars: Episode V - The Empire Strikes Back' and what roles did they play?
    GRAPH.QUERY movies "MATCH (a:Actor)-[r:Acted_in]-(m:Movie) WHERE m.movie_id = 1 RETURN a.name,m.title,r.role"

    ```
    ```redis Count Nodes
    // How many actors are in our graph?
    GRAPH.QUERY movies "MATCH (a:Actor) RETURN COUNT(a)"

    ```
    ```redis Count Relationships
    // How many 'played in' relationships are in our graph?
    GRAPH.QUERY movies "MATCH ()-[r:Acted_in]->() RETURN COUNT(r)"

    ```
    ```redis Delete Relationships
    // Delete all 'played in' relationships for Mark Hamill
    GRAPH.QUERY movies "MATCH (a)-[r:Acted_in]->() WHERE a.actor_id = 1 DELETE r"
    
    ```
    ```redis Delete Nodes
    // Delete Star Wars: Episode V
    // Note that when you delete a node, all of the node's incoming/outgoing relationships are also removed.
    GRAPH.QUERY movies "MATCH (m) WHERE m.movie_id = 1 DELETE m"

2. Properties

    ```redis Property types
    // Create a node with properties of multiple types:
    // a: a 64-bit signed integer, b: a 64-bit double, c: a string literal, d: a Boolean value, e: an array of primitive types
    GRAPH.QUERY movies "CREATE (:node {a:3+4, b:3/4, c:3=4, d: '3 4', e:[3+4, 3/4, 3=4, '3 4']})"

    ```
    ```redis Get node's property names
    // Get an array of all the node's property names
    GRAPH.QUERY movies "MATCH (n:node) return keys(n)"

    ```
    ```redis Get all property names
    // Get an array of all property names of all nodes in the graph
    GRAPH.QUERY movies "CALL db.propertyKeys() YIELD propertyKey"

3. Indexing 

    ```redis Textual match
    // All movies where Mark Hamill acted
    GRAPH.QUERY movies "MATCH (a:Actor)-[r:Acted_in]-(m:Movie) WHERE a.name = 'Mark Hamill' RETURN a.name,m.title"

    ```
    ```redis Inspect execution plan
    // Constructs a query execution plan but does not run it.
    // Inspect the execution plan to better understand how your query will get executed.
    GRAPH.EXPLAIN movies "MATCH (a:Actor)-[r:Acted_in]-(m:Movie) WHERE a.name = 'Mark Hamill' RETURN a.name,m.title"

    ```
    ```redis Create index
    // Create an index on the 'name' property of all nodes with label 'Actor'
    GRAPH.QUERY movies "CREATE INDEX FOR (a:Actor) ON (a.name)"

    ```
    ```redis Inspect updated execution plan
    // Inspect the execution plan after creating the index
    GRAPH.EXPLAIN movies "MATCH (a:Actor)-[r:Acted_in]-(m:Movie) WHERE a.name = 'Mark Hamill' RETURN a.name,m.title"

    ```
