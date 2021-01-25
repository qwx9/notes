# Indexed exact match algorithms

## Definition

The idea here is to render repeated searching in a huge text more efficient.
Frequent applications include searches (requests) on genomic sequences,
where indexes are built for much faster processing.

There are 3 types of requests, with corresponding methods:
- find P in S once
- find all occurrences of P in S
- find number of occurrences of P in S


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

To find a pattern P in T,
one simple needs to attempt to follow the path
corresponding to successive characters in P
until a leaf with its last character is reached.
Failing that, P ∉ T.


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


## Compacted tries

A non-root node of degree = 2 can be compacted
by concatenating it and the next character into a single edge.
In other words, nodes which have only one outgoing possibility
can be joined with the next node.

The idea is the same as compacted de Brujin graphs.

	r → b → a → b → c	r → bab → c
	              ↘ d	        ↘ d

Formally, a compacted trie T(V,E) rooted on r and of degree ≥ 3,
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

Compacted tries have the obvious advantage
of using less memory by reducing the number of edges,
which is the most costly part of a tree.

We consider that the number of nodes
does not depend on word size.
Here, the number of internal nodes is always ≤ 2n - 1.
The upper bound is much lower and depends solely on the number of words.


## Suffix tree

### Definition

The _suffix trie_ T of suffixes of S
is simply the trie associated to the set of suffixes of S.

The problem here is searching for any occurrence of P in S.
P is a factor of S if and only if P is a prefix of a suffix of S,
ie. P ∈ S if and only if P is "readable" in the suffix trie of S,
starting from the root.

The problem with ordinary suffix tries
and why they are not used in practice
is that they take up too much space.
What is used are *compacted suffix tries*,
and this is what is called a *suffix tree*.

Exact definition:
a suffix tree of S ∈ Σ∗ is the compacted trie of its suffixes,
with every leaf containing the _position of the corresponding suffix_.
There are other equivalent definitions.


### Search strategy

Whether it's a suffix tree or a simple compacted suffix trie,
the first step is adding a terminating character ∉ Σ,
usually _$_.
This is required because the suffix tree is correctly defined
only if no suffix is the prefix of another suffix.

![Example tree](exact.index.001.png)

	Without a terminating $:
	abaababaab
	       ---
	  ========
	aab is both a suffix and a prefix to another suffix.
	aab would not be represented as a leaf in the tree,
	which conflicts with the definition of the tree.

	…a→a→b→a→b→a→a→b
	 -----

	Adding a terminating character prohibits this situation.

	…a→a→b→a→b→a→a→b→$
	      ↘ $

Again, P ∈ S if P is readable in the suffix tree,
starting from the root.

	In the example, if we ask aba ∈? S,
	it is in tree, therefore it's true.

In addition to finding prefixes,
the P node's children are all the suffixes in S
for which P is their prefix.
This gives us directly all the positions
of P in S.

	abaababaab$
	===
	     ===--
	   ===----
	3 occurrences of aba.

In other words, if P is readable in the suffix tree,
and path leads to a node v,
the leaves of the subtree T_|v rooted at v
are the occurrences of P in S.

By definition, a tree has at most n-1 edges.
However, being that these (sub)trees
are at least binary and compacted,
the number of edges is at most 2·|leaves|.
This means that the number of edges in the subtree T_|v
is ≤ 2·|leaves|.


### Complexity

For m occurrences of P, at node v,
the number of edges in its subtree is at most 2·|leaves of T_|v|,
giving a total complexity for the search O(n + 2·ocurrences).
2·occurrences is needed to find all occurrences,
and is assumed to be small.

An uncompressed trie would have a maximum of n·(n+1)/2 nodes,
ie. O(n²),
while a suffix tree would have at most n suffixes,
therefore ≤ 2n - 1 nodes (if it's at least binary),
ie. O(n).

It has been proven that given a constructed suffix tree,
there is no better method than suffix trees
for finding a word in a text.


#### Space

There are clever optimizations possible for suffix trees,
but generally we cannot go below 20n bits.


#### Building the suffix tree

Each time a word is added,
it is sufficient to read the word in the tree,
and add it when a dead-end is reached.

In the worst case,
each of the m suffixes must be read,
hence n·(n+1)/2 operations.

Again, quadratic complexity for preprocessing.


#### Ukkonen's algorithm

This algorithm enables the tree to be constructed in linear time.
However it is extremely complex,
both for implementation, and understanding how and why it works.

Today, it is no longer necessary since there are much simpler algorithms,
essentially based on *suffix tables*,
often around O(n·logn) time.

Suffix tables have much lower space complexity,
while preserving similar search complexity.


### Applications

Being that this search's complexity only depends on the size of P,
it is a method applicable in eg. genomics,
and has provided a huge leap in performance in these domains.


### Problems

Quadratic complexity would be prohibiting in bioinformatics,
in addition to the space complexity,
even more prohibiting,
where RAM capacity is the biggest limit.

Often what is important in bioinformatics
is the constant factor multiplying the number of nodes.
Critically, besides the nodes,
edges and the words on each edge must also be stored.
While search complexity is linear,
space might still be prohibiting.
For indexing genomes, this constant must be as low as possible.

Dividing this number by 4 could be the difference
between being able to perform the computations on a personal laptop
and needing access to a cluster
(for instance 20GB vs. 4GB of RAM).

The ideal index is the tree of suffixes.
Other algorithms will attempt to approach its time complexity
while reducing the size of the index.
Ie. we won't get anything better than O(n)
for answering P ∈? S, and similar complexity for finding all occurrences.


## Pat-tree

### Definition

This is a variation of suffix trees,
where edge labels are replaced
by the first character and the size of the word.
This avoids storing large words.
Such a definition would mean that everything else is lost,
and only an gross approximate search would be possible.

S is assumed to be stored as well as the tree.
P can be disregarded quickly if P ∉ S.
If P ∈ S, it must be verified a posteriori,
which can be done by simply comparing
the remaining characters not in the tree,
by using the sizes in the nodes.

Accessing a single character in an array is O(1),
ie. complexity for this is constant
so long as random access is guaranteed.

Pat trees are the first in many other shortcuts.
Other methods store last character,
or multiple characters.


## Suffix links

### Definitions

These essentially augment suffix trees
with additional edges:
we add {u,v} if v is a suffix of u.


### Construction

These edges are easy to find if we already have the tree,
but most suffix tree construction algorithms
add these for free.


### Applications

This idea enables many additional operations
and queries on suffix trees.

One class of operations are for repeats,
such as finding the longest repeat,
repetitions in tandem,
etc.

Another application is the longest common factor
between two texts.
This had allowed the first assembly of a human genome,
ie. given two reads,
finding the longest common subsequences,
or generally given a set of reads,
find repeats,
and enable assembling the reads together.
This avoids comparing each pair of reads,
by constructing the suffix tree for each read,
and finding overlaps rapidly.

In addition, many compaction algorithms
are based on suffix trees.

Other applications include finding frequent words,
or searching for regular expressions
more efficiently that constructing its complete automaton.

It also allows fuzzy searches by allowing gaps
or mismatches,
for instance for aligning words to a genome
(local alignment).
An actual alignment algorithm would normally be necessary,
but it is costly.
We can say here whether there is a alignment in the genome
for which there are at most k mismatches,
speeding up alignment.


## Suffix trie optimization

![Suffix trie](exact.index.002.png)

![Tree after compaction](exact.index.003.png)

As we've seen before,
an easy transformation leading to huge benefits
is the transformation of the suffix trie
into a suffix tree.

However, a second possibility exists,
the transformation into a minimal DFA.


## Transformation DAWG

As seen previously,
tries can be transformed directly as a *finite-language minimized DFA*.
In other words, the automaton, like a trie,
will produce a finite set of words,
which means it has no cycles.

Suffix trees correspond to automatons
where transitions are not single characters.
They represent a finite set of words.
Minimization of the automaton is even easier here,
the crucial step is finding equivalent states,
since all states are accessible by definition of the trie.

The minimized automaton corresponding to a suffix trie
is called a *DAWG*
(directed acyclic word graph).
The term is preferred to DFA,
since the automaton is a special simpler case.

Two states are equivalent
if the subtrees rooted at the corresponding node are the same.
We can start from the leaves and traverse to upper levels
to detect them.

	In the example above,
	the states with right language
	corresponding to subtrees b→a are equivalent.
	Same with b→c.
	Here, both subtrees of a previous node in each branch
	have equivalences (ie. c→b→a and c→b→a on both sides).
	In other words the two trees containing these subtrees,
	rooted at the c nodes can also be merged.
	We could find them by starting at the subtree leaves
	and traversing backwards.

![Equivalent nodes in the trie](exact.index.005.png)
![Corresponding minimized automaton: DAWG](exact.index.006.png)

The complexity of reading a word in the automaton
is the same as the trie.
Recognizing factors can be done by considering intermediary states.
Suffixes can be found by looking at states starting from final nodes.


## cDAWG: compacted DAWGs

The idea here is to complement the result of one optimization with the other,
since suffix trees can be represented as a DFA and vice versa.

In other words, by minimizing a suffix tree,
or compacting a DAWG,
the result is a common object,
a *cDAWG*
(compacted DAWG).

DAWGs can be compacted by merging states which
with single inward and outward transitions.

![Equivalent cDAWG](exact.index.007.png)

	The suffix trie in the example has 17 states.
	Minimizing it leads to a DAWG with 9 states.
	Compacting it leads to suffix tree with 7 states.
	Both lead to a cDAWG with 3 states.

![Other example](exact.index.008.png)


### Complexity

cDAWGs greatly reduce the number of states/edges,
which is very advantageous in terms of space.
Answering P ∈ S is done the exact same way
and has the same O(m) complexity.
Getting suffixes and prefixes
works in an analogous manner to DAWGs,
since we can use final states,
and we can stop at any point during traversal.

There is a guaranteed upper bound on the number of states
(and its reduction).

There is no guarantee of reduction between tries and DAWGs,
which means they can have states in the order of n².

A suffix tree can already be compacted,
in which case there would be no gain,
but compaction from a trie guarantees that each node has a degree ≥ 3.

As a result, cDAWGs may have at most 2n - 1 states,
and 3n - 4 transitions
(like suffix trees, it does not depend on word size),
both being linear as a result.


#### Space

It is proven that this is the data structure
with the smallest space footprint
for a search in O(m) time.

There are links between S's structure and the size of the cDAWG,
determining for instance the minimum compression size.
These include entropy, Kolmogorov complexity, etc.,
ie. the capactity of S to encode information,
and its complexity/genericity.

For instance a sequence of 300 A's in a genome
carries very little information.
The more the cDAWG is large,
the more information S carries,
and the less efficiently can it be compressed.


#### Building

Building still takes linear time in practice,
but there are obvious additional steps compared to a simple suffix tree.

The preferred path is compaction to a suffix tree,
then minimization,
since there are very efficient algorithms for trie compaction,
in O(n) time.

Minimization on the other hand is much less trivial,
and common algorithms are in O(n·logn),
but it can be done in O(n).

In other words,
building can still be in linear time,
but the minimization step is non-negligable,
especially if the suffix tree is already large.


### Applications

Suffix trees are used extremely frequently
and are preferred over cDAWG despite their advantages.

Some operations are more difficult,
such as finding the number of occurrences of P.
This can still be achieved by keeping track
of the numbers of equivalent subtrees in the suffix tree
and adding them recursively.

The most problematic is finding all occurrences of P in S.
This data structure loses position information
when subtrees are merged.
Subtrees may be equivalent
but they carry this additional information in the tree's topology.

Should we augment the cDAWG with this information,
we'll simply arrive at the same space complexity as a suffix tree.

There are still many applications for the structure,
such as testing if a sequence belongs to a certain species,
something very common in metagenomics.
DAWGs are a generalized data structure not specific to suffixes.


### Summary: comparison of cDAWG vs. suffix trees

Suffix trees:

	+ number of requests
	+ positions of occurrences
	- memory footprint

cDAWG:

	+ memory footprint
	- no number or position of occurrences (requests)
	- larger build time
	- no suffix links, and other possible operations


## Searches on multiple texts

Here the problem is formalized as follows:

	Let {S₁,…,S₏} be a set of texts,
	and P a word that will be searched for in each.
	∃? i≤n such that P ∈ Sᵢ?

All of the algorithms seen previously are generalizable to this problem.

Typically, the set of texts will just be concatenated together
into a single one.
The problem there is overlaps between texts
which can produce artificial occurrences.

A possible solution is to interpose a terminating character _$_
between each as before.
Sometimes, Σ can even be augmented with differents ones for each sequence,
eg. {$₁,…,$₂}.
This is the common solution for general suffix trees
(ie. suffix trees for any number of sequences).

An example is a suffix tree of all genomic reads.
Genomes themselves are composed of multiple contigs,
chromosomes, etc.
and can be indexed in the same way.
In practice, we don't need to concatenate all the reads,
we can simply process them one by one,
adding a unique terminating character at the end of each one.


## Suffix tables

This is an important data structure still in frequent use today.


### Definition

The idea here is to conserve all or almost all the properties of suffix trees,
but with a much lower memory footprint.

A suffix table is an integer list
of the positions of the suffixes,
suffixes being ordered alphabetically.
It is assumed that the terminating character
comes before the rest of Σ.

	S = a  a  b  a  c  a  a  b  a  c  $
	    ₁  ₂  ₃  ₄  ₅  ₆  ₇  ₈  ₉ ₁₀ ₁₁

	Suffixes in alphabetical order:

	i	SA
	-----------------
	1	11	$
	2	6	aabac$
	3	1	aabacaabac$
	4	7	abac$
	5	2	abacaabac$
	6	9	ac$
	7	4	acaabac$
	8	8	bac$
	9	3	bacaabac$
	10	10	c$
	11	5	caabac$

	SA = {11, 6, 1, 7, 2, 9, 4, 8, 3, 10, 5}

SA is the table SA,
where SA[i] is the position of the ith suffix
in alphabetical order.
The only thing kept in memory will be SA.

We reuse the property that a pattern P
is a factor of S if it's the prefix of a suffix.

The first remarkable property here
is that any pattern in S always appears in an interval in SA.
Therefore when searching for P,
we look for which suffixes P is a prefix to.

	a ∈ SA[2,7] (1-indexed)
	ac ∈ SA[6,7]

Since suffixes were ordered alphabetically,
the set of suffixes of a prefix must necessarily be consecutive.

To determine if a word P is contained in S,
we find its interval in SA,
which will be 0 if it isn't.
Since SA is ordered, we can simply use binary search,
this time one for the lower bound, and one for the upper bound.

	P = bac
	We need the interval in SA with b as a prefix.
	First b is looked for in SA, starting from the middle.
	The next character in alphabetical order is c.
	We then search for c from the middle table now starting at c.

	b ∈ SA[8,9]

To then extract the suffixes corresponding the this interval,
we can simply access the positions SA[i..n] in S,
which correspond to the suffixes SA was built from.
Knowing S and SA, no information is lost.

	SA[8] = 8	bac
	SA[9] = 3	bacaabac
	These indices directly correspond to the ones in the table above.


### Interval bounds algorithms

#### Left bound

The left bound is the smallest suffix for which P is a prefix,
wrt alphabetical order, not word size.

	l ← 1
	r ← n
	while l < r			// l and r are iteratively moved until they are equal
		mid ← ⌊(l+1)/2⌋		// floor
		if P > S[SA[mid]..n]
			l ← mid + 1
		else
			r ← mid
	return l


#### Right bound

Biggest suffix for which P is a prefix.

	l ← 1
	r ← n
	while l < r
		mid ← ⌊(l+1)/2⌋		// floor
		if P > S[SA[mid]..n]
			l ← mid
		else
			r ← mid - 1
	return r


#### Search

The returned interval's positions aren't ordered,
if the first occurrence is required,
the interval must be parse.


### Complexity

#### Running time

Either of the interval bounds: O(m·logn).
m comparisons of P at each position,
for each logn steps.
Obviously, this is higher than the O(n) of the suffix tree,
but it is expected.
This can even be improved.

See also: [divide and conquer master theorem](/algorithms/complexity/asymptotic.analysis).


#### Space

We have to store S and SA.

Let's assume that numbers are stored as 32 bit integers.
Therefore the most efficient representation is: 32n bits = 4n bytes.
This is only possible however if S isn't too big.
Past 2³²-1 ≅ 4.29e9 characters,
we'd then need 64bits instead
simply to have a bigger address space,
thereby doubling space.

This is common with many bioinformatics algorithms,
such as k-mer based methods.

Compare this with ≥ 20n bits for suffix trees.
In practice, this factor of 5 can be huge.
Obviously though, the time we gain in space we lose in speed.

If we didn't have the index,
we'd have to work on the original input,
which means storing n characters.

	Note: storing characters in python,
	it would be much bigger than the suffix table,
	ie. the index would be bigger that S!

The bigger Σ, the more characters to represent.
For an alphabet Σ, the number of bits required
to store a character c ∈ Σ is log₂|Σ|.

We don't lose much by storing sequence and suffix table,
we essentially multiply by 2.

A genomic alphabet of only 4 characters,
could be stored in 2 bits, meaning 2n space.
In practice, DNA sequences are rarely encoded in 2 bits.
We have $, the EOL character, so we need at least 3 bits.
We also have N's in alignments etc.,
so now ¦Σ| = 6,
so 3n bits, etc.


### Optimizations

This data structure greatly improves space complexity,
but we would like to improve time complexity.
We can do better than the binary search comparing
the entire pattern for all logn iterations.


## Enhanced suffix table (fr. tables des suffixes enrichi)

### Definition

Improves the pattern search in the suffix table,
by adding more information for the search,
but mostly it simulates all the operations
that can be done on a *suffix tree*.

Recall that using suffix tree can be used for all kinds of problems,
whereas suffix tables can't be used for much other than pattern searching.

An additional table is constructed the *lcp table*
(longest common prefix).
Let's suppose we have this table.
Usually, it is calculated for free at the same time as the suffix table.

lcp is a table of length n
such that lcp[i] = |longest common prefix|
between suffix in position i and suffix in position i-1,
ie. between S[SA[i-1],…,n] and S[SA[i],…,n].

	S = a  a  b  a  c  a  a  b  a  c  $
	    ₁  ₂  ₃  ₄  ₅  ₆  ₇  ₈  ₉ ₁₀ ₁₁

	i	SA	S[SA[i]..n]	lcp
	-----------------------------------
	1	11	$		-1
	2	6	aabac$		0
	3	1	aabacaabac$	5
	4	7	abac$		1
	5	2	abacaabac$	4
	6	9	ac$		1
	7	4	acaabac$	2
	8	8	bac$		0
	9	3	bacaabac$	3	
	10	10	c$		0
	11	5	caabac$		1


Let M and M´ be two suffixes,
suppose we can compute "efficiently" the value of lcp(M,M´),
the longest common prefix between M and M´.

lcp only gives us the value for two consecutive suffixes.
We cannot know a priori the longest common prefix between
non-consecutive suffixes,
but we'll leave that for later.

A naive approach can build lcp by comparing
every pair of consecutive prefixes,
however let's suppose that we can do it in constant time.

Again, what we improve isn't the number of iterations,
but the binary search,
ie. avoid comparing P at each step.


### Strategy

At some step of the binary search,
we have an interval [l,r],
and we compare P with M
where M is the middle of [l,r].

	l ... M ... r

Comparing left to right,
say we observe a difference in the comparison
beginning with the k+1th character,
ie. P and M have a common prefix of size k.

Suppose also that P > M,
ie. the k+1th character of P > k+1th character of M
(we could do it the other way, it doesn't matter).
The new interval we consider is [r,M] and its middle M´.

	l ... M ... M´ ... r

What are the implications for the next iteration?
We won't need to compare all of M again.
Let's consider the question k >? lcp(M,M´),
that is, is k bigger than the longest common prefix between the two.

Example:

		        k = 4
	                ↓
	P	a c a b b a d

	l
		…
		            lcp(M,M´) = 6
		            ↓
	M	a c a b a a b
		…
	M´	a c a b a a c
		…
	r

	P and M differ at k+1 with k = 4.
	Now we'll have to search in [M,r].
	Here lcp(M,M´) = 6.

	Because lcp(M,M´) > k,
	we know that M and M´ are the same
	up until 6th index,
	while P differed at 4th position.
	Since P > M, we can directly infer that P > M´
	without comparing them.

Thus, having P > M,
if lcp(M,M´) > k,
M[k+1] = M´[k+1]
and P > M´.

Now if k > lcp(M,M´):

		      k = 3
		      ↓
	P	a a a c d

	l
	…
		  lcp(M,M´) = 1
		  ↓
	M	a a a b c
	…
	M´	a b a a d
	…
	r

	We know that P differ at k+1,
	but M and M´ differ at index 1,
	therefore P and M´ will differ at index 1.

	Since M > M´ in alphabetical order,
	we know that the mismatched character is also bigger in M´.
	
	Therefore, P < M´, again without comparing
	anything besides k and lcp(M,M´).

Thus, having P > M,
if lcp(M,M´) < k,
M < M´
and P < M´.

The last case is k = lcp(M,M´).

		        k = 4
		        ↓
	P	a a b c b

	l
	…
		        lcp(M,M´) = 4
		        ↓
	M	a a b c a
	…
	M´	a a b c c
	…
	r

	Here we know that P = M = P´ until k+1.
	We cannot decide directly if P < M´.
	Here we are obligated to test k+1th caracter
	between P and M´.

Thus, having P > M,
if lcp(M,M´) = k,
we have to compare P and M´
starting at position k+1,
during the same iteration.

k can never decrease throughout the iterations.
Therefore every character in P will be compared at most once.


### The lcp table

As previously discussed,
the lcp only specifies the longest common prefix
between consecutive suffixes,
not any arbitrary pair.

There are two approaches around this.


	S = a  a  b  a  c  a  a  b  a  c  $
	    ₁  ₂  ₃  ₄  ₅  ₆  ₇  ₈  ₉ ₁₀ ₁₁

	i	SA	S[SA[i]..n]	lcp
	-----------------------------------
	1	11	$		-1
	2	6	aabac$		0	←
	3	1	aabacaabac$	5
	4	7	abac$		1
	5	2	abacaabac$	4	←
	6	9	ac$		1	⇐
	7	4	acaabac$	2
	8	8	bac$		0
	9	3	bacaabac$	3	⇐	
	10	10	c$		0
	11	5	caabac$		1


#### Minimum lcp

The first approach is calculating min(lcp(i,j))
(i and j included),
with i and j indices in SA of two suffixes M and M´.

The intuition here is that any time
a character changes at one position,
it will be different at all subsequent
suffixes compared to the very first.

	The longest common prefix between SA[2] and SA[5] here
	is the minimum of the lcp's between them,
	ie. min(SA[2..5]).

	What about SA[6] and SA[9]?
	min(SA[6..9]) = 0.
	That simply means that the first letter
	has changed in between them.

This works, but we do have to traverse the table
between i and j each time.


##### Range minimum queries (RMQ): cartesian trees

Given an integer array,
we would like to know which is the minimum between indices i and j.
This constitutes an RMQ.

Luckily there are data structures adapted to this situation.
There are many approaches here,
but one of the most common ones are cartesian trees.

These are binary trees.
The values in i and j are first found in the tree,
then we go through their parents until a common one is found.
That will be the minimum between i and j.

	A = [3,1,5,20,2,25]

           1
	 ↗   ↘
	3     2
	    ↙  ↘
	   5    25
             ↘
	       20

	We want min(A[3..6]).
	Parent: 2
	⇒ min(A[3..6]) = 2.

To construct this,
the minimum of the array is taken,
then the table is divided in two,
the minimum of each subarray is found,
which constitutes a root for a new subtree,
and so on.

Answering a query here corresponds to a common ancestor problem.
Given two nodes of a rooted tree,
finding the smallest common ancestor is done in constant time,
using slightly more than just the tree.

The point is that there is an entire class of data structures
to answer this problem efficiently
with the usual gamut of compromises
between time and space complexity.


#### Precalculate all possible lcp(M,M´)

This brute-force approach is fairly effective.
We could construct a large n·n matrix for each pair,
but that has an obvious prohibitive O(n²) cost in space.

In practice, we don't need every single pair.
In a binary tree, there aren't that many possible ones.

For a binary search, there aren't that many possible pairs.
We compare the middle of the intervals of interest.
Recall that that's what M and M´ are to begin with.
There are potentially as many possible intervals
as there are possible M,M´ pairs.


	A = [1, 2, 3, 4, 5, 6, 7, 8, 9]
	    l                         r
	    l            l|r          r
	    l      l     l|r   r      r
	etc.

Any time we tighten an interval,
any other smaller intervals
will always be completely contained in the "parent".

Another way to look at this is that
any position in A can only be the middle
of a single interval in the binary search.
This is true regardless of the parity of the table.


	A = [1, 2, 3, 4, 5, 6, 7, 8, 9]
	           [  ↑  ]
	    [   ↑  ]
	    [            ↑            ]

Therefore regardless of what is being searched for,
the number of possible intervals in a binary search
is at most n.

Using this property is the simplest way
to precalculate all possible lcp(M,M´).


### Complexity

Thanks to the new search algorithm,
given that we make at most m comparisons for P,
the search complexity is reduced to O(logn + m).

Preprocessing is now in O(n) thanks to the shortcuts we've found.

Space complexity: O(n + n·logn + n·logn).
In theory, the size of a suffix table is O(n·logn) in the general case,
otherwise n·constant.
In the worst case, the lcp table is also n·logn.

Overall: O(n·logn).

As we've seen before, we need n integers for positions,
and thus for 32 bit (64 bit) integers, 4n bytes (8n).
Generally, we'd need log₂n bits to store all integers [1,n],
while if all integers are of constant size,
it's |integer|·n.


### Applications

Suffix table + lcp is more or less equivalent to the suffix tree,
which is why the trees aren't used much anymore,
since these also have a much smaller memory cost.


## Burrows-Wheeler Transform (BWT)

Given a word S, with a terminating character $,
a table is built with the ordered set
of all possible rotations of S.

Each column contains a single occurrence
of each character ∈ S.

The first and last columns of the table
are called respectively F and L.
This last column L is what constitutes the BWT.

	S = aabacaabac$

	 F                     L
	-------------------------
	*$* a a b a c a a b a *c*
	*a* a b a c $ a a b a *c*
	*a* a b a c a a b a c *$*
	*a* b a c $ a a b a c *a*
	*a* b a c a a b a c $ *a*
	*a* c $ a a b a c a a *b*
	*a* c a a b a c $ a a *b*
	*b* a c $ a a b a c a *a*
	*b* a c a a b a c $ a *a*
	*c* $ a a b a c a a b *a*
	*c* a a b a c $ a a b *a*

Formally, the BWT of a text S is the last column
obtained by ordering all rotations of S
in lexicographical order,
terminating character first.

Any character in F has in L
its preceding character in S,
because of the rotation.


### Link between BWT and suffix tables

Generally, the most efficient implementations of the BWT
use a suffix table.

	i	prec	suff(SA[i])
	-----------------------------------
	1	c	$
	2	c	aabac$
	3	$	aabacaabac$
	4	a	abac$
	5	a	abacaabac$
	6	b	ac$
	7	b	acaabac$
	8	a	bac$
	9	a	bacaabac$
	10	a	c$
	11	a	caabac$

With suffixes ordered alphabetically,
the column formed by the character preceding each suffix
corresponds exactly to the BWT of S.

Thus an alternative definition of BWT
is BWT[i] = S[SA[i]-1].


### Compression

Assume that a given text S has many repeats.
The corresponding BWT would then have
many identical consecutive characters.

	The word 'acgt' occurs k times in a text S.
	All the k rotations prefixed by copies of 'cgt'
	will be ordered consecutively in L.
	Same with 'gt', 't'.

	F	L
	---------
	…	…
	cgt	a
	cgt	a
	cgt	a
	…	…
	gt	c
	gt	c
	gt	c
	…	…
	t	g
	t	g
	t	g
	…	…

This is ideal for compression.
The simplest idea here would be to replace repeats
with the character and the number of repetitions.

	aaaaaabbbbbbaaaaab ⇒ 6a6b5a1b

Of course many other compression algorithms may be used.
Common techniques here are _num-length encoding_,
_move to front_,
_Huffman (aka Haufmann) coding_,
etc.

In practice BWT is typically compressed,
and many compression algorithms allow
the same operations on sequences transparently,
with the same complexity.
In fact papers on BWT usually imply compressed BWTs for this reason.


### Recovering S

A BWT, compressed or not,
does not lose information by not storing S,
since it can be used directly to recover it.

The idea here is to start from $, the last character,
and find iteratively the preceding characters.
Each character L[i] is looked up in F,
where the corresponding row j in L
directly yields L[j],
F[j]'s preceding character.

Since we only store L, the BWT,
we have to calculate a function LF(i)
(last-first),
where for LF(i) = j,
L[i] = F[j].

In other words,
the character preceding L[i] is L[LF(i)].

#### Algorithm

	i ← indexof $ in S
	S ← ""
	for k in [1,n]
		S ← L[i] · S	// strcat(L[i], S)
		i ← LF(i)


#### Computing LF

##### Element counts

Let C be an integer table of size Σ,
associating each character x ∈ Σ
with the number of characters of S
which are strictly smaller than x.

	∀x ∈ Σ, C[x] = count(∀y ∈ S where y < x)
	
	S = bbabcbbabc$

	x	$	a	b	c
	C[x]	0	1	3	9

An important benefit here is that 
this provides information on F,
that is the intervals in F for each letter.

	Here we have 4 ranges:
	[C[$]+1,C[a]], [C[a]+1,C[b]], [C[b]+1,C[c]], [C[c]+1,n]

This alone is insufficient since we need to know the precise rank
that the character is at.
Generally, the solution would be at C[c]+1+k,
where k is the rank.


##### Rank in suffixes prefixed by x

Among the k suffixes [C[x]+1,C[x]+k],
at which position is this x located?

We can represent the k suffixes as
the series of sequences X₁,…,X₏ prefixed by x,
the order of which does not depend on x (constant).

	x = b:
	bX₁	babc$
	bX₂	babcbbabc$
	…

These sequences can be retrieved directly from L.
The first characters of every X
must necessarily also occur in F.
Further, any time L[i] contains a x,
F[i]'s prefix will necessarily be an X,
since they are preceded by x.

It follows that the rank of a given x in L
is the same as its rank in F:
the ith x in L will be in the ith position
in the x-interval in F.

In other words:

	LF[i] = C[L[i]] + occ(L[i], i)
	where
		i: ith position in L
		x: character L[i]
		C[L[i]]: number of characters smaller than x in S
		occ(x,i): number of occurrences of x in L[1..i])
	and F[LF[i]] the correspondence with L[i].


### Pattern searching in a BWT

#### Strategy

The general strategy is a recursive process
whereby a pattern P will be looked for
in reverse order, character by character.

	P = acb
	1. find "b"
	2. find "cb" from 1.
	3. find "acb" from 2.

We've seen how to find the interval for a pattern of size 1,
ie. the interval for a character x:

	intv(x) = [C[x]+1, C[x₊₁]]
		with x ∈ Σ
		and x₊₁ the next character in lexicographical order

Unrolling the process:

	let p: a suffix of P
	let l(p): lower bound of intv(p)
	let r(p): upper bound of intv(p)

	S = acbbcaacbd$
	P = acb

	x	$  a  b  c  d
	C[x]	0  1  4  7 10

	1. find "b"

	1  $acbbcaacbd
	2  aacbd$acbbc
	3  acbbcaacbd$
	4  acbd$acbbca
	5  bbcaacbd$ac bX l(b)
	6  bcaacbd$acb bX
	7  bd$acbbcaac bX r(b)
	8  caacbd$acbb
	9  cbbcaacbd$a
	10 cbd$acbbcaa
	11 d$acbbcaacb

		l(b) = C[b] + 1 = 4+1
		r(b) = C[c] = 7
		intv(b) = [5,7]

	2. find "cb" from 1.

	1  $acbbcaacbd |⏸
	2  aacbd$acbbc |⏸ ←
	3  acbbcaacbd$ |⏸
	4  acbd$acbbca |⏸
	5  bbcaacbd$ac  ⏸ ← bX
	6  bcaacbd$acb  ⏸   bX
	7  bd$acbbcaac       bX
	8  caacbd$acbb       cX
	9  cbbcaacbd$a       cX cbX
	10 cbd$acbbcaa       cX cbX
	11 d$acbbcaacb

		l(c) = C[c] + 1 = 8
		r(c) = C[d] = 10
		occ(c, l(b)-1) = 1
		occ(c, r(b)-1) = 2
		l(cb) = l(c) + occ(c, l(b)-1) = 8 + 1
			why? ⇒ i = l(b), L[i] = c
			LF[5] = C[c] + occ(c, 5) = 9
		r(cb) = l(c) + occ(c, r(b)-1) = 8 + 2
			why? ⇒ i = r(b), L[i] = c
			LF[7] = C[c] + occ(c, 7) = 10
		intv(cb) = [9,10]

		From step1, b-prefixed suffixes are in [5,7].
		We want all b-prefixed suffixes preceded by c.
		We know c-prefixed suffixes are within [8,10].
		To know how many we should skip,
		we find how many c's there are in L before [5,7].
		There's one (occ(c, l(b)-1) = 1),
		hence cb-prefixes begin at 8+1 = 9.
		To know where to end, we need the occurrences of c
		before b-prefixes upper bound: occ(c, l(b)-1) = 2.
		Therefore cb-prefixes end at 8+2 = 10.

	3. find "acb" from 2.

	1  $acbbcaacbd |⏸
	2  aacbd$acbbc |⏸   aX
	3  acbbcaacbd$ |⏸   aX  acbX
	4  acbd$acbbca |⏸ ← aX  acbX
	5  bbcaacbd$ac |⏸
	6  bcaacbd$acb |⏸
	7  bd$acbbcaac |⏸
	8  caacbd$acbb |⏸
	9  cbbcaacbd$a  ⏸ ← cbX
	10 cbd$acbbcaa       cbX
	11 d$acbbcaacb

		l(a) = C[a] + 1 = 2
		r(a) = C[b] = 4
	        occ(a, l(bc)-1) = 1
		l(acb) = l(a) + occ(a, l(bc)-1) = 2 + 1
			i = l(cb), L[i] = a
			LF[9] = C[a] + occ(a, 9) = 3
		r(acb) = l(a) + occ(a, r(bc)-1) = 2 + 2
			i = r(cb), L[i] = a
			LF[10] = C[a] + occ(a, 10) = 4
		intv(acb) = [3,4]

#### Algorithm

	x ← P[m]
	l ← C[x] + 1
	r ← C[x+1]
	for i=m-1 to 1
		x ← P[i]
		l ← C[x] + 1 + occ(x, l-1)      // l(x) + occ(x, l(P[i+1])-1)
		r ← C[x] + occ(x, r)            // l(x) + occ(x, r(P[i+1])), with r: upper bound+1
		if l > r
			return "absent"
	return l, r


### Complexity

#### Time

We need to know the number of occurrences of a character
at a position in the BWT.
In the loop, this is needed 2m times.

Therefore search operations: 2m * [time for occ(x,i)]

The questions we're able to answer with just this
are whether P ∈ S and its number of occurrences in P,

It is possible to have occ() run in constant time,
thus these requests are in O(m) time.


##### rank/select and wavelet trees

In practice, occ is implemented using _succinct data structures_,
specifically _rank/select_.

Suppose Σ is just {0,1}.
Ranking is answering rank_x(i),
ie. the number of occurrences of x until position i.
Select is answering select_x(n),
ie. where is located the nth occurrence of x.

rank/select structures are very light in memory.
We need them to be basically the same size as the bit table,
and capable of answering both rank and select operations
in constant time.

In our case, occ isn't a bit vector,
ie. Σ is bigger than just {0,1}.
There are generalizations for any Σ,
such as _wavelet trees_.

The point of all of these approaches
is to build a data structure that takes
more or less the same space as the sequence S
and where rank and select operations are in constant time,
or at the very least amortized constant time.


##### Positions of all occurrences

We can't directly find
the positions of all occurrences.

We saw how to move backwards starting from $,
but we can use the LF function on any character
to find its position from L to F,
ie. its preceding character.
Once we find the $, we have located the character in S.
The problem there is that we're essentially doing this
for each character in *S*.
Say the character is at position 3e9 of a genome,
we'll need to do this 3e9 times,
which defeats the purpose.

If we really need to find the position,
we have to store the positions of the elements of the BWT,
but that's an array the size of S,
in which case we might as well use a suffix tree.
In fact, we'd be storing the actual suffix table.

In practice, we make a compromise:
storing only some of the positions,
which is the idea behind FM indices.
It's a trade-off which can be tweaked
any time an FM-index-based tool is used.


#### Space

We have to store C, which is O(|Σ|).

We also have to store the BWT,
which is nothing but a permutation of S,
and thus takes the same amount of memory:
n·log₂|Σ|,
eg. 2n for |Σ| = 4.

S itself does not need to be stored,
since the BWT can losslessly recover it.

However, a compressed BWT
can take much less space than S itself.

Hence overall O(|Σ| + n·log|Σ|)


## FM index

### Definition

FM-indices are based on the BWT,
with additional information for some positions.

	S = acbbcaacbd$
	                pos
	1  $acbbcaacbd   ?
	2  aacbd$acbbc   ?
	3  acbbcaacbd$   1
	4  acbd$acbbca   ?
	5  bbcaacbd$ac   ?
	6  bcaacbd$acb   ?
	7  bd$acbbcaac   9
	8  caacbd$acbb   3
	9  cbbcaacbd$a   ?
	10 cbd$acbbcaa   ?
	11 d$acbbcaacb   ?

For a given character,
if its position wasn't stored,
we "simulate" the backtracking reconstruction of S
where we look for the preceding characters,
until we find a position that is stored,
from which we infer the initial character's position.

This just prevents us from having to reconstruct all of S
each time.

There are several strategies for selecting
which positions to store.
We could store every nth position,
or select them randomly for n total positions.
The more the positions, the more efficient the search,
but the more memory is required,
but at least there's a knob we can turn
to adjust to our needs.


### Complexity

In terms of space, like BWT we store the transform
and the C table,
but we add a pos table.

Search time for k occurrences is in the order of: O(m) + k·O(n/|pos|).
O(m) for finding intervals in the table,
and for each occurrence,
the cost of backtracking to a known position.

If we chose positions at random,
we have the same expression, but with an expected value
(fr. espérance).
