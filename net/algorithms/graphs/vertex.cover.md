# Vertex cover

## Definition

	let G = (V,E) be a graph
	let S ⊆ V
	S is a vertex cover if ∀e ∈ E, e is incident to a vertex ∈ S
	ie. S includes at least one endpoint of every edge in E

|S| is termed the _cost_ of the vertex cover S.
The vertex (or node) cover problem consists of finding the cover of minimum size,
which is a classical NP-hard problem with an existing approximation algorithm.
Its decision version is said to be NP-complete.


### Minimum vs. minimal

Care should be taken not to confuse the two (well gee).

A _minimum_ vertex cover means that the solution is of minimum size.

A _minimal_ vertex cover means that it cannot be reduced,
ie. there is no subset of the minimal vertex cover that is also a vertex cover.

A vertex cover can be neither or both of these,
and finding a _minimum_ vertex cover is harder.


## Greedy vertex cover algorithm

	let G = (V,E) be a connected graph
	SOL ← ∅
	Remaining ← E
	while Remaining ≠ ∅
		v ← vertex incident to the maximum number of edges in Remaining
		SOL ← SOL ∪ {v}
		Remaining ← Remaining - {any e ∈ Remaining incident with v}
	return SOL

The algorithm does produce a _minimal_ vertex cover,
ie. one that cannot be reduced in size,
but not the _minimum_ vertex cover,
ie. the optimal solution.

	Example:
		  / B - E
		A - C - F
		  \ D - G
	The algorithm will choose {A}, then {E,F,G}, producing a cost of 4.
	However, the optimal solution is {B,C,D}.


### Question: is it a 2-approximation algorithm?

The common way for such problems is to consider that GreedyVC is a minimisation problem,
and to attempt to show ρ > 2 in order to infirm the idea.

Let S be a vertex cover.
By definition, for each edge, at least one of the endpoints is part of S.
Let G be a bipartite graph where all vertices of one partition have degree = 3.
The first few vertices of the other partition will have degree = 3,
then at some point degree = 2, and the rest will have degree = 1.

Since in the case of equality, the algorithm chooses a vertex arbitrarily,
we could assume it always chooses from the second partition when this occurs.
We'll end up selecting all the vertices in the second partition,
starting with those with degree = 3, then 2, then 1.
By illustrating this visually, it is easily apparent that the optimal solution
is all of the vertices in the first partition, of which there are much less.

To generalize this problem, we'll set a degree k for all nodes in the first partition.
There are k! vertices in the first partition.
The second partition will have k!/k vertices of degree k,
k!/(k-1) vertices of degree k-1, k!/(k-2) vertices of degree k-2,
etc. ending with k!/1 vertices with degree 1.

Generalizing this, ∀1 ≤ i ≤ k, the second partition will have k!/(k-i) vertices of degree i,
and k colors.
Every vertex in the first partition will be adjacent to 1 vertex of each color in partition 2,
which satisfies d_G(v) = k ∀ v ∈ part1.

As shown before, the algorithm could choose all vertices from the second partition,
while the optimal solution is the vertices from the first (k!).
In other words:

	ALGO/ALGO∗ = (k! * (1/k + 1/(k+1) + ... + 1) / k!
	for k sufficiently large, (1/k + 1/(k+1) + ... + 1) ≅ k!logk
	ALGO/ALGO∗ ≅ k!logk/k! = logk

Therefore, not only is the algorithm not a 2-approximation,
but its error factor is not constant:
the larger the graph (k), the bigger the error,
and we will never have a constant approximation.

If the size of the error depends on the size of the graph,
and we have no constant upper bound ρ since it depends on k,
and the approximation is arbitrarily bad.
This is very unsatisfactory.


## Two for the price of one algorithm

	let G(V,E) be a connected graph
	SOL ← ∅
	E´ ← E
	while E´ ≠ ∅
		let {u,v} be an arbitrary edge ∈ E´
		SOL ← SOL ∪ {u,v}
		E´ ← E´ - {e incident to u or v}
	return SOL

It is a 2-approximation.


### Proof

This runs in polynomial time,
where in the worst case, the entire graph is traversed.
Accessing edges can be done in constant time with eg. adjacency matrices.

Vertex pairs added by the algorithm are a set of disjoint edges,
since adjacent vertices are removed for each.
The optimal algorithm must cover all of these edges,
and therefore pick one of the two endpoints of each edge.
It follows that the optimal solution
is at least half the size of this algorithm's solution.
Therefore the approximation ratio is at most 2.

This can be shown to be true even in the case
of the previous k! bipartite graph.


## Better solutions

_Karakostas, 2009_ proposes a better approximation ratio for the VC problem,
with α ≅ 2 - 1/√logn.

_Hastad, 2001_ posits that
there is no α-approximation algorithm for this problem
with α < 7/6
unless P = NP.
