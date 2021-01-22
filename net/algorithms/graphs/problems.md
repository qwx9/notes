# Problems and exercises

## Produce an minimum weight connected subgraph with k vertices

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
	It *would* work if we looked at all the edges
	instead of selecting a random vertex,
	and it would still be in polynomial time.


## Maximum spanning tree

Consider the following modification to Prim's algorithm:
choose the maximum weight edge at each step instead of the minimum weight one.

Does the algorithm produce a spanning tree?

	Since G is connex, intuitively the result would be a spanning tree
	regardless of the optimization problem.
	Prim's algorithm produces a spanning tree, which would still hold.

Does the algorithm produce a maximum spanning tree?

	Intuitively, the optimization problem is just flipped, and the
	algorithm works just like before.
	At the first step, a local maxima is chosen, and by recurrence,
	a global maxima will be arrived at.
	The same proof could be used either by flipping the conditions,
	or by changing the sign of all weights:
	since the two problems are shown to be equivalent,
	resolving one also resolves the other.
	In fact, the same proof holds if we have weights in ℤ,
	since nothing stipulates there being a particular sign.


## Greedy maximum matching

Show that the first algorithm correctly produces a maximum matching
if the graph is acyclic (forest of trees).

	Generalizing to a forest is trivial
	if we can prove this for a single tree.

	By definition, a tree's leaves have degree = 1.
	To prove this,
	let v be any vertex ∈ V,
	and p a path = {v,u₁,…,u_k}.
	No vertex can appear more than once in p,
	otherwise there would be a cycle.
	Therefore subgraphs cannot be extended indefinitely.
	If the path cannot be extended beyond u_k,
	then d(u_k) = 1.
	In addition, any graph must have at least two leaves
	for it to be a graph.
	Therefore any tree would have at least 2 vertices of degree = 1.
	Further, a tree is a connected graph,
	and there can be no vertices of degree = 0.

	The algorithm will therefore start with leaves.
	Suppose a leaf {u,v} ∉ M, the maximum matching.
	If we add {u,v} to M, only one vertex now has two incident edges.
	One of the edges of M can be deleted to attain
	a maximal matching containing {u,v}.

	For M to be a maximum matching, M must contain an edge incident to u or v,
	otherwise {u,v} could be added to M and it would not be maximum.
	Say we add {u,v}.
	v has now two edges in M ∪ {u,v}: {u,v} and {v,w}.
	(M - {v,w}) ∪ {u,v} is a new maximum matching containing {u,v}.

	Therefore, it is certain that ∃ a maximum matching containing {u,v},
	and the algorithm works for trees.
	Finally, a forest being a set of trees,
	the algorithm also works for forests.
