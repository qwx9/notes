# Indexed exact match algorithms

## Definition

The idea here is to render repeated searching in a huge text more efficient.
Frequent applications include searches (requests) on genomic sequences,
where indexes are built for much faster processing.

There are 3 types of requests, with corresponding methods:
- find W in S once
- find all occurrences of W in S
- find number of occurrences of W in S


## Tries

### Definition

Tree with a root r,
and edges labeled with a character Σ.

	T is a trie = (V,E), Φ: E → Σ,
	where Φ is a labeling function for each E,
	associating a character to an edge.

A word in a trie is a word
read along a path from the root to a leaf.
Words recognized by the trie can be listed with a simple DFS.
Obviously the root is not part of a word.

Nodes are associated by a character via their incoming edge
(rather than the outgoing one).
A leaf node has a single incident edge with the final character.

A simple trie like this only serves to represent a set of words.


### Searching

To find a pattern W in T,
one simple needs to attempt to follow the path
corresponding to successive characters in W
until a leaf with its last character is reached.
Failing that, W ∉ T.


### Complexity

Searching: O(m).
To attain this complexity,
getting the next node in the sequence must be in constant time,
as in with hash tables.
Regardless of the size of the trie,
the complexity will only depend on m.

Constructing and updating the trie is very straightforward.
From a set of words, the trie can be built
either depth-first (word by word)
or breadth-first (by level).

Tries can take a lot of space,
and storing a string is much less expensive
than a path in a graph,
since the path must store extra information
besides the characters themselves.

In other words,
the trie will have as many leaves as there are words,
and both vertices and edges must be stored.
In the worst case, the number of internal edges
is the sum of the word sizes.
The number of edges has a greater effect on space complexity
than the words themselves.


### Applications

Tries are commonly used to represents sets,
as in python.
Unions and intersections are also trivial.

Integer sets can be represented as sequences of (binary) bits.
An integer is transformed to a pattern in binary,
which is then searched for.
Therefore for a 64bit integer,
the search is bound to 64 nodes at most.


### Relationship with finite state automata

A trie can easily be transformed to a DFA,
by considering every node as a state,
the root as the initial state,
leaves as final states,
and labels the transitions between states.


### Prefixes

Given a trie T, we can directly get the prefixes of any word
by reading its path partially.
In other words, every internal node corresponds to a prefix,
with leaves a complete prefix, the entire word.


## Compressed tries

A non-root node of degree = 2 can be compacted
by concatenating it and the next character into a single edge.
In other words, nodes which have only one outgoing possibility
can be joined with the next node.

The idea is the same as compressed de Brujin graphs.

	r → b → a → b → c	r → bab → c
	              ↘ d	        ↘ d

Formally, a compressed trie T(V,E) rooted on r and of degree ≥ 3,
ie. has all internal nodes except r of degree ≥ 3,
ie. they all have one incoming edge, and least two outgoing ones,
and a label function now Φ: E → Σ∗,
ie. not the alphabet but any word ∈ Σ.


### Search

Same as before,
but when traversing the trie,
all characters must be tested in the compacted edges.


### Complexity

Same as before: O(m).

Compressed tries have the obvious advantage
of using less memory by reducing the number of edges,
which is the most costly part of a tree.

We consider that the number of nodes
does not depend on word size.
Here, the number of internals nodes is always ≤ 2|M| - 1.
The upper bound is much lower and depends solely on the number of words.


## Suffix trie

### Definition

The suffix trie T of suffixes of S
is simply the trie associated to the set of suffixes of S.

The problem here is searching for any occurrence of W in S.
W is a factor of S if and only if W is a prefix of a suffix of S.
