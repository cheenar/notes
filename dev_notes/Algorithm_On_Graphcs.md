...menustart

 - [Algorithm On Graphs](#311ad33f7584ac17012490ce8852f7e8)
	 - [Week1](#3c88e16de2066fa3ce3055a55a3e473b)
		 - [Representing Graphs](#030495a245248ce0deed9c13f9576cd0)
		 - [Exploring Graphs](#6d620d4a966ddb637a736ea4670b6782)
			 - [Explore , starting from a Node](#78f63ae6b311ad607db08a56dd79649a)
			 - [DFS](#c1bb62b63c65be3760b715faad0bdf8d)
		 - [Connectivity](#9bd9d0ebc081bd74f5bef4e136bb1aed)
			 - [Connected Components](#8a4d6dec35ad6f01d54531c509cf7d37)
		 - [Previsit and Postvisit Orderings](#cb7d104acd7ef78260476fb96e632beb)
			 - [Previsit and Postvisit Functions](#c190b4dd3a865a419b7d42ecdded4b14)

...menuend


<h2 id="311ad33f7584ac17012490ce8852f7e8"></h2>

# Algorithm On Graphs

<h2 id="3c88e16de2066fa3ce3055a55a3e473b"></h2>

## Week1

<h2 id="030495a245248ce0deed9c13f9576cd0"></h2>

### Representing Graphs

 - Edge List
    - egdes: (A, B), (A, C), (A,D), (C,D) 
    - A,B,C,D are vertices
 - Adjacency Matrix
    - Entries 1 if there is an edge, 0 if there is not.
 - Adjacency List
    - For each vertex, a list of adjacent vertices.
    - A adjacent to B, C,D
    - B adjacent to A
    - C adjacent to A,D
    - D adjacent to A, C
    - can be implemented by a dictionary 
 
 - Defferent operations are faster in defferent representations
 - for many problems , want adjacency list .

Op | Is Edge? | List Edge | List Nbrs 
--- | --- | --- | ---
Adj. Matrix | Θ(1) |  Θ( &#124;V&#124;²) |  Θ(&#124;V&#124;)
Edge List | Θ(&#124;E&#124;) | Θ(&#124;E&#124;) |  Θ(&#124;E&#124;)
Adj. List | Θ(deg) | Θ(&#124;E&#124;) | Θ(deg)


<h2 id="6d620d4a966ddb637a736ea4670b6782"></h2>

### Exploring Graphs

 - Visit Markers
    - To keep track of vertices found: Give each vertex boolean visited(v).
 - Unprocessed Vertices
    - Keep a list of vertices with edges left to check.
    - This will end up getting hidden in the program stack
 - Depth First Ordering
    - We will explore new edges in Depth First order. 

<h2 id="78f63ae6b311ad607db08a56dd79649a"></h2>

#### Explore , starting from a Node

 - Need adjacency list representation!

```python
def Explore(v):
    visited(v) ← true
    for (v, w) ∈ E:
        if not visited(w):
            Explore(w)
```

<h2 id="c1bb62b63c65be3760b715faad0bdf8d"></h2>

#### DFS

```python
def DFS(G):
    for all v ∈ V : mark v unvisited
    for v ∈ V :
        if not visited(v):
            Explore(v)
```

---

<h2 id="9bd9d0ebc081bd74f5bef4e136bb1aed"></h2>

### Connectivity

<h2 id="8a4d6dec35ad6f01d54531c509cf7d37"></h2>

#### Connected Components

 - Explore(v) finds the connected component of v
 - Just need to repeat to find other components.
 - Modify DFS to do this.
 - Modify goal to label connected components

```python
def Explore(v):
    visited(v) ← true
    CCnum(v) ← cc
    for (v, w) ∈ E:
        if not visited(w):
            Explore(w)

def DFS(G):
    for all v ∈ V mark v unvisited
    cc ← 1
    for v ∈ V :
        if not visited(v):
            Explore(v)
            cc ← cc + 1
```

---

<h2 id="cb7d104acd7ef78260476fb96e632beb"></h2>

### Previsit and Postvisit Orderings

 - Need to Record Data
    - Plain DFS just marks all vertices as visited.
    - Need to keep track of other data to be useful.
    - Augment functions to store additional information

<h2 id="c190b4dd3a865a419b7d42ecdded4b14"></h2>

#### Previsit and Postvisit Functions


```python
def Explore(v):
    visited(v) ← true
    previsit(v)
    for (v, w) ∈ E:
        if not visited(w):
            Explore(w)
    postvisit(v)
```

 - Clock
    - Keep track of order of visits.
    - Clock ticks at each pre-/post- visit.
    - Records previsit and postvisit times for each v.

![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/algr_on_graph_previst_clock_example.png)

 - Computing Pre- and Post- Numbers
 - Initialize clock to 1.

```python
def previsit(v):
    pre(v) ← clock
    clock ← clock + 1
def postvisit(v):
    post(v) ← clock
    clock ← clock + 1
```

 - Previsit and Postvisit numbers tell us about the execution of DFS.
 - Lemma
    - For any vertices u, v the intervals pre(u), post(u)] and [pre(v), post(v)] are either **nested** or **disjoint**.
        - nested: eg.  (1,8) , (2,5)
        - disjoint: eg.  (1,8) , (9,12)
    - that is , Interleaved (not possible) 
        - eg. ( 1, 8 ) , ( 5, 9  )

---

## Week2 

### Directed Acyclic Graphs

 - Directed graphs might be used to represent:
    - Streets with one-way roads.
    - Links between webpages.
    - Followers on social network.
    - Dependencies between tasks.


#### Directed DFS

 - Can still run DFS in directed graphs.
    - Only follow **directed** edges
    - explore(v) finds all vertices **reachable** from v.
    - Can still compute pre- and postorderings.

#### Cycles

 - A **cycle** in a graph G is a sequence of vertices v1, v2, . . . , vn so that
 - (v1, v2),(v2, v3), . . . ,(vn−1, vn),(vn, v1) are all edges.
 - ![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/algorithm_on_graph_cycles.png)
 - Theorem
    - If G contains a cycle, it cannot be linearly ordered.

#### DAGs

 - A directed graph G is a **Directed Acyclic Graph** (or DAG) if it has no cycles.
 - Theorem
    - Any DAG can be linearly ordered


### Topological Sort


 - Last Vertex
    - Consider the last vertex in the ordering. It cannot have any edges pointing out of it
    - ![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/algorithm_on_graph_last_vertex.png)
 - Sources and Sinks
    - A **source** is a vertex with no incoming edges.
    - A **sink** is a vertex with no outgoing edges
        - ![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/algorithm_on_graph_sinks_in_dag.png)

#### Finding Sink

 - Question: How do we know that there is a sink?
 - Follow path as far as possible v1 → v2 → . . . → vn. Eventually either:
    - Cannot extend (found sink).
    - Repeat a vertex (have a cycle).

#### TopologicalSort Algorithm

```python
TopologicalSort(G)
    DFS(G)
    sort vertices by reverse post-order
```


### Strongly Connected Components

 - Two vertices v, w in a directed graph are **connected** if you can reach v from w and can reach w from v.
 - Theorem
    - A directed graph can be partitioned into **strongly connected components** where two vertices are connected if and only if they are in the same component.
    - ![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/algorithm_on_graph_SCC.png)

#### Metagraph

 - We can also draw a **metagraph** showing how the strongly connected components connect to one another 
    - ![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/algorithm_on_graph_metagraph.png)
 - Theorem
    - The metagraph of a graph G is always a DAG.
 
How to compute the strongly connected components of a graph. ?


### Computing Strongly Connected Components

 - Problem
    - Input: A directed graph G
    - Output: The strongly connected components of G. 

#### Sink Components

 - Idea: 
    - If v is in a sink SCC, explore(v) finds vertices reachable from v. This is exactly the SCC **of v**.
        - 因为SCC的性质，无论你从SCC中的任何一个点node开始，你都是 explore 真个SCC
    - that also means, you will get different SCC, if you start from different node.
 - Need a way to find a sink SCC.
    - why ? 
    - if you start from a source SCC, you many visit the whole graph, it won't help you.


#### Finding Sink Components

 - Theorem
    - If C and C' are two strongly connected components with an edge from some vertex of C to some vertex of C' , 
    - then largest post in C bigger than largest post in C'.
 - Conclusion
    - The vertex with the largest postorder number is in a source component!
    - Problem: We wanted a sink component

##### Reverse Graph Components

 - Let Gᴿ be the graph obtained from G by reversing all of the edges.  
    - ![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/algr_on_graph_reverse_graph.png)
 - Gᴿ and G have same SCCs.
 - Source components of Gᴿ are sink components of G.  
 - Find sink components of G by running DFS on Gᴿ .
    - The vertex with largest postorder in Gᴿ is in a sink SCC of G.

#### Algorithm

```python
def SCCs(G):
    run DFS(Gᴿ)
    for v ∈ V in reverse postorder:
        if not visited(v):
            Explore(v)
            # mark the learder node as that SCC
            mark visited vertices as new SCC
```

 - Runtime
    - Essentially DFS on Gᴿ and then on G.  
    - Runtime O(|V| + |E|).


