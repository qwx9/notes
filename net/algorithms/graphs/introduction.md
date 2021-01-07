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


## Spanning tree

### Example problem

Given a limited budjet, find the minimum number of roads to renovate in a village,
that is the minimum acyclic set of edges E´ which covers V,
and with minimum total weight.
A cycle would mean two possible paths exist.
This is a minimum spanning tree.


### Definition

A spanning tree is a subgraph and a tree G´ = (V,E´)
which covers all V.
There are many such trees for each graph.
A spanning tree may be constructed with simple DFS or BFS.


## Minimum spanning tree (MST)

An MST of a weighted graph G is the spanning tree of minimal total weight.
French: arbre couvrant minimum (ACM).

	let T = (E´,V) a spanning tree
	let w: E → R weight of the graph
	w(T) = ∑w(e) ∀e ∈ E´
	An MST is a tree T where w(T) is minimal.

Here a DFS or BFS do not guarantee correct results
since they do not take weights into account.


### Kruskal algorithm (1956)

The idea is to choose step by step a minimum weight edge
within remaining edges of E
which would not create a cycle within currently selected edges.
This continues until no edges remain or any new edge would create a cycle.

Edges are first ordered by ascending weight
to avoid parsing an edge list at each step.
At each step, the head of the list is popped.

In a naive approach, a BFS or DFS on current subgraph
is used to check for a cycle with the new edge.
If one is created, the edge is discarded.

A more elegant solution is to consider that a cycle is not created
if and only if the new edge's vertices are elements of distinct connex components,
where the set of current edges determine connex components.
Adding an edge where both vertices are elements of the same component
would necessarily create a cycle.

	let G = (E,V) a weighted connected edge graph
	SOL ← ∅
	E ← 3-dimensional array ∀e = {x,y} ∈ E with elements {x,y,w} of vertices x,y and weight w
	ORDER E according to weights in non-decreasing order
	for each u ∈ V do
		C[u] ← u				// each vertex its own connected component at first
	end for
	for each i=1 to m do
		{u,v} ← E[i].x, E[i].y			// head of list according to weight
		if C[u] ≠ C[v] then
			SOL ← SOL ∪ {u,v}
			c ← C[v]
			for each w ∈ V do
				if C[w] = c then
					C[w] ← C[u]	// merge components
				end if
			end for
		end if
	end for
	return SOL

Ordering E: O(mlogm).
Assigning C for each v ∈ V: O(n).
Since an MST is a tree, we can have at most n-1 merges,
so the if will be true n-1 times,
and the cost of the inner loop is n: O(n(n-1)).
The outer loop will at most cost this merge n times,
and nothing m-1 cases,
so both loops: O(n(n-1) + m-1).
Sum: O(mlogm + n + n(n-1) + m-1) = O(mlogm + n²) since n and m-1 are linear.

Using a disjoint-set data structure like disjoint-set forests with union ranks,
we have O(mlogm) sorting + O(mlogm) total for loop = O(mlogm) total.
Because m is at most O(n²) (n(n-1)/2 edges for a complete graph),
we can also say it runs in O(mlogn), which is nice since in most cases n < m.


### Prim algorithm (1957)

At each step, we find the edge of minimum cost that extends the current tree.

	SOL ← ∅
	choose a (any) vertex s in G
	C ← s
	while C ≠ V do
		let {u,v} the edge of minimum weight among E with u ∈ C and v ∉ C
		SOL ← SOL ∪ {u,v}
		C ← C ∪ v
	end while
	return SOL

The loop occurs n times since a vertex is added at each iteration,
there is always an edge to add since the subgraph is connex.
Finding a vertex to form an edge with takes n steps to check all external vertices.
Therefore, Prim runs in O(n²) overall.

For graphs with few edges, Kruskal is better.
Prim is easy to express, but difficult to implement efficiently.
Using a binary heap and adjacency list over an adjacency matrix and searching,
complexity can be reduced to O((n+m)logn) = O(mlogn).
Using a fibonnaci heap can further drive complexity down asymptotically to O(m + nlogn)
when the graph is dense, and for very dense graphs,
a d-ary heap can run it in linear time.
