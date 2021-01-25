# k-mer indexing methods

## Definitions

Here we answer the query P ∈? S
for P of fixed size,
ie. k-mers.

Obviously other indexing approaches such as BWT can be used,
but the fact that k is constant
allows using much more efficient algorithms in this case.

The most naive approach is the number of possible factors in S.
When k is not constant, it is in the order of n²
(start and end position can each vary n times),
and we cannot afford to store them all.

When k is constant, there are at most n-k kmers in S,
ie. O(n).
Here it is possible to store them all in eg. a dictionary,
which works when S isn't too big.


## Seed/chain/extend

These methods are what most recent mappers are based on.

First, seeds are looked for.
This can be an exact search of for instance a short read
on an entire genome.
Chaining attempts to find the number of colinear seeds.
Finally, extending the seeds is attempted.


### Seeding

Most mappers will vary in their definition of a seed,
and its method of detection.

There two types of seeds:
fixed-size (k-mer),
and maximum-size (ie. unbounded: mem).

Mem's attempt to find the longest possible word
which matches exactly between read and genome.
Methods for mem's are essentially the same
as the general case with indexed search,
and we basically use FM indices.

With k-mers, regardless of what sequences follow or precede them,
we just look for fixed-size words.

It is also possible to look for inexact seeds,
but in any case, exact matches are looked for.
For instance, we could take neighboring kmers
with only a few mismatches,
and attempt to find those.

We'll focus on k-mers.
There is a big difference here
between the data structures capable of answering P ∈ S,
and those that are not.
Other indexing methods usually answer this,
but here we might not be able to.


## Hash functions

We want to answer P ∈? S in constant time.
We find all possible k-mers in P and put them in a dictionary.

	P = aaccatta and we set k = 3.
	⇒ {aac, acc, cca, tta}

### General definition

A hash function of a set E
returns an integer for any given element ∈ E,
ie. h: E → ℕ.

Computer architecture is such that
in-memory representations of data can essentially
only be arrays of integers,
ie. we get an integer value for at an address in memory.
A sequence of characters will be stored as a sequence of integers
with a corresponding memory address.

A simple hash function is therefore
the transformation of a character sequence
into a representation in binary.

	f → ℕ
	where a = 0, c = 3:
	aac → 0b000011 = 3

We can then just make a big table
in which we store the needed values.
Should we just encode a set,
with values true or false for presence/absence,
we can just store 1s and 0s.
Here a 0 would mean a kmer is not part of the set.
Each table element would correspond to a certain encoded value.

	kmer	aaa     aab
	hash	000000  000001
	val	0	1

### Naive approach: basic hash function

Given a set of kmers,
a hash function will transform any given kmer
into an integer index
referencing an element in a bit table,
with a true or false value,
for whether or not the kmer is part of the set.

Initially the vector is empty,
meaning the hash table is set to 0s.
When parsing all kmers of the sequence to be indexed,
their hashes are computed,
and the corresponding elements of the hash table are set to 1.

Testing if a kmer is part of the set works the same way.
Its hash returns an index to the table,
and the value in the table determines
whether that kmer is present or not.


### In practice

This approach works for a small k.
However, as k increases,
so does the size of the table,
which must represent all possible values of the kmer.
For k=32, we'd need 2³²⁺¹ values, ie. a table of size 2⁶⁴.

Obviously, in reality most values would be 0,
we'd never have 2⁶⁴ different kmers.

What we would like is to transform kmers into values [0,n].


## Closed hash tables

We want E --f-→ [1,n].
We can simply use a modulo operator:

	hash = K mod n
		where K is the integer representation of a kmer

The computed hashes will then always be in [0,n[,
and the hash table will now be of size n.

The problem now is that there will be hash collisions,
ie. the function may produce the same hash for multiple kmers.

As an example, python uses dynamic hash tables,
where n is initially at a fixed value,
and grown progressively.
A python dictionary could store integer values
labeled by a string key.
The table actually contains lists
to handle hash collisions,
which is what is referred to as _closed hash tables_.

	dict = {                 hash    table
	"aca": 2,                0       0 ["aca":2]
	"aaa": 3,    ---f--->    1   →   1 ["aaa":3, "atc":4]
	"aac": 1,                2       2 ["aac":1]
	"atc": 4                 1   
	}

Hashing is usually in constant time,
but if multiple values can be stored for each hash,
looking up the right value is in linear time.
We are also forced to store store the keys themselves in the table.
This is also true for testing non-existant elements,
ie. "alien membership".

This is the simplest way of handling collisions,
but is not optimal for kmers,
where we could do much better.

Storing the keys is usually the reason
for a high cost in terms of space.


## Minimal perfect hash functions


### Definition

This is a class of hash functions for a fixed size set E,
ie. it cannot be constructed dynamically,
and is _injective_,
ie. there cannot be collisions.

	∀x,y ∈ E with x≠y, f(x) ≠ f(y).

The impossibility of a collision constitutes a _perfect_ hash function.

The function is _minimal_ when its size
is bounded by the number of elements in the set,
or is at least in the same order.
Many minimal perfect hash functions
will store slightly more information.


### Construction

There are many methods, but the simplest one is trivial.
An empty vector A₀ of size |E| is made.
A hash function h₀ is chosen arbitrarily.
It needn't be perfect.
The vector will store 1s if no two elements have the same hash,
or 0 in case there is a collision.

Next, the subset E of elements with colliding hashes are taken.
A new vector A₁ of size |E₁| is made.
A new hash function h₁ is used,
and here again there is a risk of collision,
and 1s and 0s are stored accordingly.

This is repeated as long as there are still collisions,
in the hope that by the 3rd iteration, there aren't any.
Finally, the vectors A₀,…,A₏ are concatenated.


### Look-up

Given an element e ∈ E,
its hash is calculated with h₀.
If the corresponding value is 1, we're done.
Otherwise, we must look in A₁ with h₁,
and so on.

Once a 1 is arrived at,
its rank is evaluated,
just like with the BWT,
and that is used as the hash.

For example:

	A₀[h₀(e)] = 0
	A₁[h₁(e)] = 1
	hash = rank(A₁[h₁(e)])

The maximum number of 1s in the concatenated table A
is |E|,
hence the rank is always < |E|.

This final hash is used to access a table which
contains directly the values for a single element.

This creates a very efficient associative table,
particularly well adapted to kmers,
and with a smaller footprint since keys are not stored.

The hash functions here are of the form:

	h(x) = (a·x + b) mod p) mod |E|
		where p is a sufficiently large prime number

Storing the hash functions themselves
usually just means storing the a and b parameters,
and maybe p.


### Application

This implements an associative tables for kmers,
capable of storing kmer counts,
doing set operations
as in metagenomics (associating a sequence to a genome),
or RNAseq (associating a read to a gene),
and finally kmer positions.

However P ∈? S cannot be answered
since keys are not stored in the hash tables.
This only makes sense if P ∈ S already.
In fact, the function will necessarily return a result
even for a non-existant kmer.

This is the approach implemented in the BBhash tool
and similar.


## Bloom filters

### Definition

Here the question is the opposite of minimal perfect hash functions.
It must answer P ∈? S.
We cannot return lists of positions here,
for which associative tables are better.

This is one of the first and most used techniques
in what is referred to as _approximate or probabilistic data structures_
(fr. structures de données approximatives).
These are data structures that could return false positives
(but never false negatives).

In other words,
non-membership is certain,
but positive membership has a chance of error.


### Construction

It is a bit vector initially set to 0.
A hash function h₁ returns the hash of a given kmer
as an index into the vector to be set to 1.

To answer a query P ∈? S,
we simply hash P and look whether the value there is 1 or not.
A 0 is certain to mean that P ∉ S.
A 1 on the other hand means P ∈ S but with a risk of error,
depending on the properties of the hash function
wrt collisions.

In practice, multiple hash functions are used to improve performance.
Hashes by every function are calculated for each kmer,
meaning that any time a kmer is added,
all elements of the table corresponding to the hashes
are updated with a 1.


### Look-up

P ∈ E is true simply if ∀h A[h(P)] = 1.
If some of the values are 0, we are certain that P ∉ E.


### Performance

Multiplying the number of hash functions
lowers the false positive rate,
but increases memory requirements,
ie bigger vectors.


### Applications

Many methods are based on this technique.
One important field of application has been De Brujin graphs.

De Brujin graphs store consecutive k-mers
with a common prefix/suffix of size k-1.
They are not stored in memory as graphs,
but a set of kmers,
and this is where bloom filters enter.

The degree of any node is at most |Σ|.
Since outgoing edges share the node's suffix as their common prefix,
we can simply perform set operations to determine
which nodes and corresponding edges exist.
While there are |Σ| operations per kmer,
this has rendered DBG very practical and efficient,
esp. in terms of memory.

Today, bloom filters are often used as a preliminary step
before a more costly operation,
to eliminate negatives quickly.
A further method may for instance determine with 100% confidence
whether the kmer exists or not.

To detect sequencing errors,
a common approach is to find infrequent kmers,
for instance unique ones.
If sequencing cover is good enough,
it usually indicates an error.
To eliminate unique kmers,
we can use bloom filters to filter the first n occurrences,
etc.


## Minimizers

### Definition

An s-minimizer of a k-mer P with s < k
is the smallest s-mer of k in alphabetical order.

	P = tcaacactaaacg, k = 13
	            ----
	4-minimizer: aaac, the smallest (first) 4-mer in P
	in alphabetical order.


### Strategy

This can be used to extract a kmer signature
to distribute them in different subsets.

	For instance, given a set E of ATGC 10-mers,
	we can separate kmers in subsets based on their 3-minimizer,
	ie. into 4³ subsets: aaa, aac, aag, aat, etc.

These subsets can then be stored on disk.
Then, to query a kmer, its minimizer is evaluated.

This enables the use of smaller hash functions.

What used to be done before
was for instance to take the first n characters of each kmer.
However, there is a big chance of needing many more (unique) buckets.
By contrast, a sequence generally has few (unique) minimizers.


### Minimap

Minimap (and minimap2) is
one of the most important recent applications.
Its implementation is a bit different:
instead of storing all the kmers of the sequence,
only the minimizers are.
This yields approximate results, similar to bloom filters.

For a sequence S of which we would like to store all k-mers,
we'd rather s-minimizers.
Then, rather than P ∈? S,
we test s-min(P) ∈ set of s-min(S).
Critically, only s-mers for s < k are stored.

Like with bloom filters,
a negative response is certain to mean P ∉ S,
but the reverse is not true.

Here an associative table with positions in S
for each minimizer is made.
In essence, minimap uses the minimizers as seeds,
which index minimizers instead of kmers.

It is known that the s-mers are a good approximation of k-mers,
thus the false positive rate is not very high.
Here, if we get a positive answer,
we have its position and we can test if it's really true,
in which case we don't even have false positives.


### Applications

The first use in bioinformatics of minimizers
has been for kmer counting,
when there is insufficient space
for all kmers.

Again in genomics,
minimizers are not taken by alphabetical order,
since it is a very bad choice there.
AAA would be a frequent 3-minimizer,
but there is a huge number of such repeats,
such as polyA tails.
The idea remains the same regardless of the order function.
