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
if the graph is acyclic (tree).
