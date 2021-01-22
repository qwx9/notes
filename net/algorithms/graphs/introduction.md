# introduction to graph theory and basic algorithms

## Definitions

### Terminology

	let G = (V,E) be a graph with V the set of vertices and E the set of edges
	let n = |V|, the graph's size
	let m = |E|, the number of edges

Vertices and/or edges may carry certain properties.
Graphs may be weighted, directed.


### Paths

Two vertices connected by a single edge are called adjacent.

Simple paths or cycles (no duplicates):

	let V´ = {v₁,...,v₏} be a sequence of unique vertices
	where each (vᵢ,vᵢ₊₁) ⊆ E and is also unique

A cycle is a path ending on its first vertex.

Two vertices are connected if there exists a path between them.
In directed graphs, the path between them must be directed.
A *connex graph* has every pair of its vertices are connected.

A graph may be composed of one or multiple connex components.
In the worst case, when no vertices are connected,
there are |V| connex components.


### Distance and degree

	let the distance d(u,v) be the size of the shortest path between vertices u and v,
	i.e. the number of edges separating them

	let the degree d_G(v) of the vertex v be the number of v's adjacent vertices


### Handshaking lemma

Every finite undirected graph G has an even number of vertices with odd degree,
i.e.:

	∑v∈V d_G(v) = 2|E|


### Special graph topologies

#### Complete graph

The *complete graph K₏*, or clique, is an undirected graph
where every vertex is adjacent to all others.
Each edge is formed by 2 points from the set of n points (n over 2).
n possibilities to choose from for the first vertex, n-1 for the second.
Pairs in reverse order are dismissed, thus /= 2.
Therefore:

	|E| = n(n-1) / 2

#### Bipartite graph

A bipartite graph G(V1, V2, E) has the set V divided into two partitions V1 and V2
where no vertices in the same partition are adjacent
and only connected to the other partition.


#### Cyclic graph

A cyclic graph can be odd (C_2k+1) or even (C_2k)
(of odd or even length).


#### Tree

A tree is a connected acyclic graph,
and the minimal connex graph.
A tree of n vertices has n-1 edges.


#### Star graph

A star graph S_k has one root central vertex connected to all others.


### Subgraphs

	let G´ = (V´,E´) be a subgraph of G(V,E)
	V´ ⊆ V, E´ ⊆ E

A spanning subgraph is a connex subgraph spanning all vertices.
An induced subgraph contains all edges E´ adjacent to vertices V´,
e.g. induced by the set V´.


## Representation

### Adjacency matrix

An adjacency matrix is a square matrix M[n,n]
where rows and columns are all v ∈ V,
and each element {i,j} has value 1 if (vᵢ,vⱼ) ∈ E,
and 0 otherwise.

This representation is redundant for an undirected graph
since the lower and upper triangle are symmetrical.

For directed graphs, either both dimensions of the matrix are used,
or another possible value (2) is added if vⱼ→vᵢ ∈ E,
with value 1 meaning vᵢ→vⱼ ∈ E.

O(n²) complexity in space even if only using lower or upper triangle.
O(n) complexity for calculating a vertex degree.
O(1) complexity for checking if an edge exists.


### Adjacency list

An adjacency list is a vector of size n,
in which each element is a vector of adjacent vertices for a unique vertex ∈ V.

O(n + m) complexity in space: n + sum of degrees, which is 2*|E|, thus O(n + 2m),
but O(n²) for a complete graph.
Faster degree calculation.
