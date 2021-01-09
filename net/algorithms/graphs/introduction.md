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
	for each u ∈ V
		C[u] ← u				// each vertex its own connected component at first
	for each i=1 to m
		{u,v} ← E[i].x, E[i].y			// head of list according to weight
		if C[u] ≠ C[v] then
			SOL ← SOL ∪ {{u,v}}
			c ← C[v]
			for each w ∈ V
				if C[w] = c then
					C[w] ← C[u]	// merge components
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
	while C ≠ V
		let {u,v} the edge of minimum weight among E with u ∈ C and v ∉ C
		SOL ← SOL ∪ {{u,v}}
		C ← C ∪ v
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


### Greedy algorithms

Uses local optima.
This is the case with Kruskal and Prim which both choose the minimum on each step
to provide a hopefully optimal global solution.
The choice of locally optimal solutions is termed *greedy choice property*.
A greedy algorithm reduces each problem into a smaller one by making one greedy choice after another.
Therefore it never reconsiders its choices, which is the main difference with dynamic programming,
which is exhaustive and guaranteed to find a solution.
Dynamic programming chooses according to all previous choices
and may reconsider the path to a solution.

A template of greedy algorithms is as follows:

	while SOL is not complete,
		x ← best element in remaining input
		SOL ← SOL ∪ x
		remove x from remaining input
	return SOL

Greedy algorithms are simple to express and implement,
but do not guarantee optimal results for many problems.
In the case of MST, both Prim and Kruskal can be proven
to arrive at a globally optimal solution by choosing locally optimal solutions.

A proof by induction (recurrence) works by proving a statement at step 1,
and showing that if it holds at step i, it will hold at step i+1 as well,
which proves it holds at any step.
This can be used in Prim's case.


#### Prim correctness

	let G be a connected weighted graph.
	at every iteration, an edge (u,v) must be found
	that connects a vertex in a subgraph to an external vertex.
	since G is connex, there will always be a path to every vertex.
	let T be an MST of G.
	if T∗ = T, then T∗ is an MST.
	otherwise, let {u,v} ∉ T be the first edge added during construction of T∗,
	and let V´ be the set of vertices connected by the edges added before e.
	then u ∈ V´ and v ∉ V´ (or vice versa).
	since T is a spanning tree of G there is a path in T joining u and v.
	traversing the path, an edge e must exist connecting a vertex in V to one not in V.
	at the iteration when {u,v} was added to T∗, e could have been added instead but was not,
	therefore w(e) ≥ w({u,v}).
	let T∗∗ be the graph obtained by removing e and adding {u,v} to T.
	then T∗∗ is connected, has the same number of edges as T, and w(T∗∗) ≤ w(T),
	therefore it is also an MST of G and contains {u,v}
	and all edges added to it during construction of V.
	by recurrence, a spanning tree identical to T is obtained, therefore an MST.

Proof by contradiction:

	assume S is not minimum weight.
	let V = (e₁,...,e₏) be the ordered sequence of edges chosen by Prim,
	and U a minimum-weight spanning tree containing edges from the longest possible prefix of V.

	let eᵢ = {x,y} ∉ U be the first edge added to S,
	and let W = (e₁,...,eᵢ₋₁) be the set of vertices immediately before {x,y} is selected.
	it follows that W ∈ U while eᵢ ∉ U.

	there must be a path x→y in U, so let {a,b} be the first edge on this path,
	with a ∈ W and b ∉ W.
	let T = U + {{x,y}} - {{a,b}}.
	T is a spanning tree for G.

	if w({a,b}) > w({x,y}), w(T) < w(U) which is impossible since U is an MST.

	if w({a,b}) = w({x,y}), T is also an MST,
	and since Prim has not selected {a,b} yet,
	{a,b} ∉ W, which implies W ∪ eᵢ ∈ T,
	which is a longer prefix of V than U contains, contradicting the definition of U.

	if w({a,b}) < w({x,y}), {a,b} will be selected by the algorithm,
	which contradicts the definition of {x,y}, the first edge added to S.

	since all three cases are contradictions,
	the original assumption that S is not an MST is invalid.
	QED.


### Exercises

#### Produce an minimum weight connected subgraph with k vertices

Consider a modified version of Prim:

	let G = (V,E), s ∈ V a vertex and k an integer.
	SOL ← ∅
	A ← {s}
	while |A| < k
		{a,b} ← edge of minimum weight where a ∈ A and b ∉ A
		SOL ← SOL ∪ {{a,b}}
		A ← A U {b}
	return SOL

If k = |V|, it is the equivalent of finding the MST of G.

Does this produce a minimum-weight tree on k vertices?

	As long as k ≤ |V|, then by definition, yes.
	At each step we create a connected acyclic subgraph, ie. a tree.

Is the algorithm correct?

	Let T be a spanning tree of G with k vertices.
	If k = |V|, s can be chosen at random since T is a spanning tree,
	and Prim's algorithm guarantees that T is an MST.
	If k < |V|, there are multiple solutions for T,
	and the induced graph T can be of heigher weight depending on the choice of s.


## Vertex cover

