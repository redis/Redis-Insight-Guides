RedisGraph is a Redis module that enables creating and querying **property graphs**.

The term property graph refers to both a mathematical structure and a data model.

**The property graph mathematical structure consists of**
* A set whose elements are called **vertices** (**nodes**).
* A set whose elements are called **edges**.
* A function that maps each edge to an ordered pair of vertices.
* Each vertex has a (possibly empty) set of nominal attributes called **labels**.
* Each edge has a nonempty nominal attribute called **label**.
* Each vertex and each edge has a (possibly empty) set of attributes called **properties**.
* Each property is an ordered pair: the property’s name and the property’s value. For each vertex and each edge, all property names are pairwise distinct.

**The property graph data model comprises the following concepts:**
* An **entity** is a physical, conceptual, virtual, or fictional particular.
* Each entity has zero or more **label**.
* A **relationship** is an association or an interaction between a pair of entities or between an entity and itself.
* Each relationship has a nonempty **type**.
* Each entity and relationship has a set of **features** (characteristics). Each feature has a name and a value. For each entity and each relationship, all feature names are pairwise distinct.

**The property graph data model comprises the following structure:**
* All entities and relationships are organized in a single property graph mathematical structure.
* Each vertex represents an entity. The vertex’s set of labels represents the entity’s labels.
* Each edge represents a relationship between the pair of entities. The edge's label represents the relationship's type.
* Properties represent features of entities and relationships. 
* For each entity and relationship, property names are pairwise distinct strings, each identifying a feature’s name, and each property value represents the feature’s value.
* Property values may be 64-bit signed integers, 64-bit doubles, string literals, Boolean values, or arrays containing any of those.

Property graphs (or simply **Graphs**) can be queried using a subset of the Cypher query language with proprietary extensions.
