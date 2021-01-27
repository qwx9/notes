# Spanning tree

## Example problem

Given a limited budjet, find the minimum number of roads to renovate in a village,
that is the minimum acyclic set of edges E´ which covers V,
and with minimum total weight.
A cycle would mean two possible paths exist.
This is a minimum spanning tree.


## Definition

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


#### Complexity

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

Therefore, in total: O(m·logn + n²).

If m is in the order of n,
the graph is called _sparse_ because it contains "few" edges,
and the term that dominates is n².
However, if m is in the order of n² (example: m = n²/4),
the graph is named "dense",
and the complexity will be O(n²·logn + n²) = O(n²·logn).
If m is an order of magnitude bigger than n²/logn,
the dominant term is m·logn.
This is why m·logn cannot be removed
and the total complexity cannot be simplified to O(n²).


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

Another analysis from _bsinaimeri_: using a min-heap structure,
we have O((n+m)·logm).
If the graph is _dense_,
m is in the order of n,
in which case the time complexity of min-heap is smaller than simple array
(O(n·logn) < O(n²)).
If the graph is _sparse_,
m is in the order of n²,
in which case a simple array is more efficient
(O(n²·logn) > O(n²)).


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


## Colored spanning tree

Given a graph G with colored edges,
find a spanning tree for which
the number of distinct colors on its edges is minimal.


### Greedy proposition

	SOL ← ∅
	A ← E
	while A ≠ ∅
		c ← most frequent color in A
		while ∃ edges ∈ A with color c
			{u,v} ← edge ∈ A with color c
			if {u,v} does not form a cycle with edges ∈ SOL
				SOL ← SOL ∪ {{u,v}}
	return SOL


#### Correctness

Kruskal-inspired algorithm that will produce
a spanning tree (acyclic).

Either the algorithm is correct and we have to prove it (difficult),
or it isn't and we have to provide a counter-example
(and draw the graph).

Let G be a clique with 15 edges (6*5/2) with blue edges.
Add one vertex with red edges connect to all others.
The algorithm will look for the red vertex eventually
and will have to add a red edge,
so it returns 2 colors,
when we could've only chosen only all red edges.


#### Approximation ratio

We saw that we had ALG/ALG∗ = 2 for the previous graph,
but can we do worse?

Let's do the same thing but add a second clique of a different color
connected to the red vertex.
Here again we'll end up with 3 when ALG∗ = 1.
We'll never get a constant factor.

Taking k monochromatic cliques of n vertices,
we'll have 2 among n (n over 2) edges of a color in each clique.
If k is constant, nk = n·(n-1)/2,
but if it isn't constant we'll have to define it...
