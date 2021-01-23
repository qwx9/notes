# Exact string search

## Definitions

	1-indexing
	Σ,A	alphabet
	Σ∗	language: set of possible words ∈ Σ (∗: repetition, like regex *)
	S	text sequence ∈ A
	n=|S|	length of S
	P,W	word
	m=|W|	length of word
	S[i]	ith character
	S[i:j]	substring between i and j
	S[1..i]	prefix
	S[j..N]	suffix
	lower case symbol: character ∈ Σ
	upper case symbol: word ∈ Σ∗
	UV	concatenation of U,V ∈ Σ∗
	Ua	concatenation of U ∈ Σ∗ and a ∈ Σ

Subword or substring is allegedly ambiguous in literature,
and factor is preferred here.
A subword may refer to any sequence of characters ∈ S in no particular order,
whereas a factor is a set of consecutive characters.

	P is a factor of S if ∃i,j ≤ |S| such that P = S[i..j].
	notation:
	P ∈ S if P factor of S

One can find the set of possible factors corresponding to any word.
ε, the empty word is always part of it.

	let P,S ∈ Σ∗, |P| ≤ |S|
	P ∈ S ?


## Objective

Main problem: test if P ∈ S.

P is usually very small and S very big.
It is then assumed that processing P would be fast,
and the methods here will mainly focus on optimizing their dependence on |S|.
Having found an occurrence, the algorithms will usually yield its position.

A second objective is to find all occurrences of P within S.
An easier third objective is the number of occurrences.

Complexity of algorithms involving preprocessing
is the sum of the complexity of preprocessing and that of the search.


## Naive algorithm

	i = 1
	while i ≤ n - m + 1
		j = 1
		while j ≤ m and W[j] = S[i+j-1]
			j++
		if j = m + 1
			output i
		i++

The use of a sliding window is apparent here and
is the basis for the simplest algorithms.
i is the position of the sliding window.

Another way to write it:

	i = 1
	j = 1
	while i ≤ |S|
		if P[j] = S[i]
			i++
			j++
			if(j == |P| + 1)
				output i - j + 1
		else
			i = i - j + 2
			j = 1

This second version uses i as the current character of S being compared,
which corresponds to the way most of the following algorithms are written.


### Complexity

Complexity: O((n - m + 1) * m) = Θ(n * m)

A simple worst-case scenario is S = aaaaaa...aa and P = aaab.

How much time to search for similarity between two complete 3Gbp genomes?

	1. cut first genome in pieces of 50bp ⇒ 6e6 words to search
	2. search each piece in second genome ⇒ 50 * 3e9 comparisons
	⇒ 9e18 operations
	for 10¹² operations per second, circa 250 hours of compute time.

Each position in the text is compared multiple times.


## Rabin-Karp

String matching algorithm that works well in practice
and can be generalized to similar problems,
such as bidimensional words.

The algorithm uses elementary number theory notions,
such as equivalency of two numbers modulo a third.

Let A be expressed in a base d = |A|.
S can then be viewed as a sequence of numbers in base d.

	⇒ with A = {0..9},
		S = 3 1 4 1 5 corresponds to 31415 in base 10.

Let w the base d equivalent of W[1..m]
and s the base d equivalent of S[t+1..t+m].
A match at t implies that s = w.

If we can compute w in Θ(m) time,
and all values s in Θ(n - m + 1),
we can then determine all the values of t by comparing w to each s
in Θ(m) + Θ(n - m + 1) = Θ(n).
We'll assume for now that the numbers are not exceedingly large.

	we note Θ(n - m + 1) since there are n - m + 1 different values for t,
	and asymptotically, if m = n, computing a single s value takes Θ(1) time, not Θ(0).

w can be computed in Θ(m) time using Horner's rule:

	w = W[m] + d * (W[m-1] + d * (W[m-2] + ... + d * (W[2] + d * W[1]) ... ))

s0 can be calculated in the same way from S[1..m] in Θ(m) time.
To calculate remaining values s1, s2, ..., sn-m in Θ(n - m) time,
we shall observe that st+1 can be calculated from st in constant time:

	st+1 = d * (st - d^(m-1) * S[t+1]) + S[t+m+1]
	
	subtracting 10^(m-1) * S[t+1] removes the highest order digit in s;
	multiplying by d shifts the number one position to the right;
	adding S[t+m+1] adds a new lower order digit.

	⇒ m = 5, st = 31415, d = 10, S[t+m+1] = 2:
	st+1 = 31415 - 10e5 * 3
		1415
	st+1 *= 10
		14150
	st+1 += 2
		14152

The constant d^(m-1) can be precalculated in O(log m) time,
but often a direct approach in O(m) suffices.

	⇒ See: Cormen et al, 31.6 powers of an element, pp 878

If the value is precalculated,
then the equation above takes constant time.
Thus, we can compute w in Θ(m) time and s0,s1,...,s_n-m in Θ(n - m + 1) time.

An additional problem persists:
w and s can be too large for simple computation.
If W contains m characters,
we cannot assume that each arithmetic operation on w (m digits)
takes constant time.
Calling again upon techniques in number theory,
a simple solution is to calculate w and s modulo an appropriate integer q.

Observing that each of the w, si, and the si+1 equations
can all be computed modulo q,
w can be calculated modulo q in Θ(m) time,
and all s can be calculated modulo q in Θ(n-m+1) time.

Let q be a prime number such that d*q fits into a machine word.
All calculations can then be done in single precision arithmetic.

The recurrence formula is adjusted to be valid modulo q:

	st+1 = (d * (st - (d^(m-1) % q) * S[t+1]) + S[t+m+1]) % q

Let h = d^(m-1) mod q:

	st+1 = (d * (st - h * S[t+1]) + S[t+m+1]) % q

This solves the problem of dealing with large numbers,
but introduces the possibility of collisions:
while st ≠ w % q implies st ≠ w,
st ≡ w % q does not imply st = w.
We can therefore use st ≡ w % q as a fast heuristic test
to eliminate mismatches quickly.
For any t where st ≡ w % q,
we have to explicitely compare W and S[t+1..t+m+1].
With a big enough q, we can hope that this rarely occurs,
and that the cost of the full comparisons is low.


### Hashing and prime numbers

The step of computing w and s0 is essentially
computing a hash of W and S[t+1..t+m] using Horner's rule and modulo q.
The computation of st+1 in constant time from st is called a rolling hash.
The efficiency of the rolling hash function above is allegedly similar
to the Rabin fingerprint rolling hash function,
which is what many implementations of the algorithm rely on.

The practice of using a hash function modulo a primer number,
thus hash tables of prime size,
is to make the frequency distribution of its output more uniform.
A uniform distribution is important for a hash function for it to be useful.
Intuitively, input should be mapped to the broadest possible set of values
in a uniform manner to avoid collisions as much as possible.
Natural language text in particular does not have a uniform frequency distribution.
q being a primer number, a lot of other numbers will be relatively prime to it,
thus making the output more uniformly distributed.

The hash function above is weak, which is why the modulo prime is important.
It would not be necessary for a stronger hash function,
where a simpler modulo factor could be used,
such as a power of two, thus using cheaper bitwise operations.
However, hash functions that are both strong
and cheap when recomputing a hash on every input character
are very difficult to design.

Note that this division might be expensive on modern hardware,
where multiplications and other operations are cheaper and processors
are better equipped to handle them.


### Implementation

As we've seen, q must be prime
and should be large enough to reduce the chances of collision.
Many copypasta implementations choose 101 as a prime number for text comparisons.

However, given that the hashes are not stored anywhere,
why not use something much bigger?

http://monge.univ-mlv.fr/~lecroq/string/node5.html
uses a different rolling hash function,
with d = 2 and q the maximum value for an integer + 1 (2³³),
to take advantage of the property common on most hardware
that integer arithmetic forgets carries (an overflow acts as a modulo).
This simplifies calculating h, removes the need of a modulo,
and reduces most operations to shifts.

Is this correct?
Whether the integer is signed or not doesn't matter.
The hash values will range between 0 and 0xffffffff.
In the above, we stipulate that d*q must be contained in a machine word,
which is no longer the case.
However, that does not matter,
since we never really have to retain intermediary results larger than an integer.

2³³ is not a prime number,
and I don't know if d not being the size of the character set
impacts anything in particular.
Best would be to measure...

Here each character has an integral value ranging from 0 to 255,
rather than a base 256 digit,
so d wouldn't actually be an issue,
since we do have a single value for each character.

Remains the question of uniform output.


### Complexity

Preprocessing: Θ(m)

Search: Θ((n - m + 1) * m) in the worst case,
but mean execution time, averaging some hypothesis, is better.

	if W = x^m and S = x^n, i.e. both have the same values everywhere,
	each position is a match and must be checked explicitely,
	which is the worst case.

Most of the time, we could expect that
only a constant number c of matches will be produced.
In that case, the expected time is O((n - m + 1) + c*m) = O(n + m).

We can do a heuristic analysis on the basis that
the reduction of values modulo q
works as a stochastic transformation
between the character set Σ* and Zq.
This is a difficult to formalize and prove problem (see Cormen 11.3.1).
We can then expect that the number of mismatches is O(n/q),
since the probability that some s is equivalent to p, modulo q,
is estimated at 1/q.

Let ν the number of matches.
Since there are O(n) mismatches and O(m) processing time for matches,
the total expected time for the Rabin-Karp algorithm is now:

	O(n) + O(m * (ν + n/q))

If ν = O(1) and if q ≥ m,
this time is O(n).
In other words, if the number of occurrences is low,
and that the chosen q is such that it is bigger than the pattern size,
we can hope that the algorithm takes only O(n + m) time,
and since m ≤ n, the expected time would be O(n).


### References
- Cormen et al, 11.3.1: using division for hashing,
wrt the discussion on expected complexity


### Applications

Generally, other algorithms with better performance like KMP and Boyer-Moore
are used to single pattern matching.

However, Rabin-Karp can easily be extended to multiple pattern searching,
where combined with the use of a set data structure like a bloom filter,
it is allegedly an algorithm of choice.
A search of k patterns would optimally result in O(n+k*m) expected time,
assuming the hash comparison is in constant time.
The Aho-Corasick algorithm has deterministically O(n+k*m) time.


### Knuth-Morris-Pratt

The KMP algorithm is actually a slightly improved Morris-Pratt algorithm,
but relies on the same idea.

Where the brute-force method must compare W and S[j] at every position j,
depending on where the mismatch occurs in W, we may restarting comparing
at an index in W higher than 1.

	If a mismatch occurs between W[i] and S[j+i] (with 1<i≤m),
	then W[1..i-1] = S[j..j+i-1].

	Let U = W[1..i-1] = S[j..j+i-1],
	a = W[i], b = S[j+i], with a ≠ b.

		      v    i
	W´	    [V]c[V]a…        V is a border of U
	W	    [   U ]a…
		    ↑======≠
	S	   …[   U ]b…
		    j      j+i

		           i
	W	        [U]c…        we must restart at W[v+1] =? S[j+i]
		           ↑         but there's no border V of U
	S	       …[U]b…
		    j      j+i

		           i
	W	            …        we can restart at W[1] and S[j+i+1]
		            ↑
	S	          …b…
		    j      j+i

Let V be a suffix of U of length v.
It is possible that V is also a prefix of W.
In that case, instead of directly restarting comparison at W[1] and S[j+1],
we may instead restart at W[v] and S[j+i],
skipping this prefix we already know matches in both W and S.

	• j is never decreased, ie. we never go backwards in S
	• if mismatch, we just don't want to start again with W[1] at S[j+1]
	• best case: there is no border ⇒ U can be skipped over entirely
		∗ we know there can be no correspondence in W[1..i]
		∗ skip U over S, start again at W[1] and S[j+i+1]
	• otherwise we must jump backwards in W
		∗ W may still match at S[j+i]
		∗ skip V over W and start again at W[v+1] and S[j+i]
	• if i=1, there can be no border (no proper prefix)
		∗ we note this as -1 in the jump table
		∗ so if i = T[i] = -1, increment i and j to restart at W[1] and S[j+i+1]

We may thus precalculate a jump table T which for each position i in W
yields the length of the largest proper suffix of W[1..i-1] that is also a prefix of W,
corresponding to the index we may jump to, or -1 if there is no such suffix,
in which case we must resume at W[1] and S[i+j+1].
This proper suffix is also called a border (occurs at both ends of U).
A proper suffix of a string is not equal to the string and is non-empty.

	T[i] is the length of the longest border of W[1..i] for 1<i≤m.

Simple description:

	i = 1
	j = 1
	while j ≤ n
		if W[i] = S[j]
			i++, j++
			if i = m + 1
				output j - i + 1
		else if i = 1
			j++
		else
			i = T[i-1] + 1

Another one with 0-indexing:

	i = 0
	j = 0
	while j < n
		if W[i] = S[j]
			i++, j++
			if i = m
				output j - i
				i = T[i]
		else
			i = T[i]
			if i < 0
				i++
				j++


### Preprocessing: constructing the jump table T:

The jump table is sometimes noted π, or lps (longest prefix/suffix).

	Example:

		a  a  b  a  a  c
		0  1  0  1  2  0

		a  b  a  b  a  c  a
		0  0  1  2  3  0  1

Since a mismatch at the very start of W is a special case,
where no backtracking is possible,
we just leave T[1] at 0.
For i>1, if there is no border in W[1..i], we set T[i] = 0,
else the length of the longest border.

With 0-indexing, we can use T[0] = -1 for some shorter code.

A naive algorithm can be O(m³).

We may use the same search algorithm to construct T in O(m) space and time,
comparing W with itself.

The insight is that at each position i,
one needs to consider checking suffixes of size x+1
only if a valid suffix of size x was found at T[i-1],
and should not bother to check x+2, x+3, etc.

Example:

	--------------------------v            is u part of a border?
	========u         ========             the previous position was, so if v = u, T[i] = T[i-1]+1
	===w ===          ==== ===             the one before also was, so if v = w, T[i] = T[i-2]+1
	x                                      we went all the way down to i=1, if v ≠ x, no border

Simple recursive algorithm:

	T[1] = 0
	i = 2
	j = 1
	while i ≤ m
		if P[i] = P[j]
			T[i] = j
			i++, j++
		else if j > 1
			j = T[j-1] + 1
		else
			T[i] = 0
			i++

Note that the pseudocode on wikipedia doesn't produce expected results.
With 0-indexing:

	T[0] = -1
	i = -1
	j = 0
	while j < m
		while i > -1 and W[i] ≠ W[j]
			i = T[i]
		i++, j++
		if W[i] = W[j]
			T[j] = T[i]
		else
			T[j] = i

### Complexity

Complexity of search: O(2n - 1) = O(n)

We advance in S n times.
Any time there's a match with W[i], its position in S is remembered,
and rollback can occur at most i times.
Imagine the word is the same as S until the very end,
and that we have to roll back n times after n comparisons.
At most, we will get 2n - 1 comparisons.
Each character in S can be compared to W more than once,
but the number is bounded by m.

In other words, in the worst case, we jump backwards,
but we still skip at least 1 character,
therefore we cannot move P more than n-m times.
The complexity of the search itself does not depend on m,
the size of the word.

Complexity of preprocessing: O(2m - 1) = O(m)
Same explanation as above.
For the first implementation,
it is easy to see that in each case,
either one or both indices are incremented,
therefore at worst, we'd have 2m - 1 iterations.

Overall complexity: O(n + m).


### Global performance

Complexity of the algorithm is O(n + m) time with O(m) space.
It is invariant regardless of how many repetitive elements there are in W or S.
There are dozens of variants of KMP.
Some have O(m*n) complexity at worse but work better than KMP in practice.
Each variant is designed for a particular application.

KMP has historic value but is not used much in practice.
In our case, preprocessing the text is much more effective than preprocessing the word,
hence the natural transition to sorted dictionary approaches.


## Boyer-Moore

Implemented frequently, as in tools such as grep(1).
In theory, it has worse worst-case scenario performance than KMP,
but in practice, it works well.
The idea once more is to construct a jump table for P to maximally avoid comparisons.

Here the sliding window of P is moved left to right,
but comparisons are made right-to-left.


### The badchar rule

We compare W and S right to left.
On mismatch, we look backwards in W for the first occurrence of S[j],
and jump the sliding window to align them.

	Example 1:
		               ↓
	S	a  a  b  a  c  a  b  a  c  d  a
	W	   b  a  c  b  c  b  a
		      ←  ←  ←  ←  ←  ←
		      ↑

		               ↓
	S	a  a  b  a  c  a  b  a  c  d  a
	W	            b  a  c  b  c  b  a
		               ↑

	Example 2:
		                     ↓
	S	a  a  b  a  b  a  a  c  a  b  c  a  b  c  b  b  c
	W	      b  b  a  a  b  b  a  b  c
		   x  ←  ←  ←  ←  ←  ←

		                     ↓
	S	a  a  b  a  b  a  a  c  a  b  c  a  b  c  b  b  c
	W	                        b  b  a  a  b  b  a  b  c

This strategy is particularly efficient when |Σ| is large,
since the maximum probability of having a character c ∈ Σ gets lower and lower.
This is the case for natural languages.

In the best case, W is not in S, we would get O(n / m) time,
eg. we can get sublinear complexity,
ie. we don't even read all the data (S) since many positions are unread.

Sometimes, since the second rule is more difficult to implement,
only this one is used because of sufficient performance in practice.

Implementation-wise,
the naive method is to parse W from start to end
and for every character c ∈ Σ,
to find occurrences of c.
Here the size of the table is in the order of m·|Σ|,
since we need all positions of c in W.
In other words, for each position in W,
we have the position of the first occurrence of each c
backwards in W.

A better alternative in both runtime and memory
is to save the position of the last occurrence of c in W,
for each c ∈ Σ.
In this case, on mismatch, we would look for the last of occurrence of c in W,
rather than the next one going backwards.
Should c occur to the right of the mismatch, we cannot jump,
and we even might have to move the window _backwards to the left_.
Here the size of the table is in the order of |Σ|,
since we only store the position of the last occurrence.
Despite its relatively lower performance,
this is usually the preferred method.


### The good prefix rule

There are at least 3 ways to implement this rule,
even though the algorithm is referred to as Boyer-Moore in all cases.
Some variants ignore it.

While comparing right to left, 
having matched a factor U until a mismatch c,
with U prefixed with a given character x,
we want to find another occurrence of U further back
with a different character prefix y.

We then slide the window forward to align the prefix
with the mismatch.

	S	----------------------c-----------
	W	          ---y====----x====
		               U        U

These occurrences are precalculated.
However, it doesn't make sense to build a table
for any character c, esp. with large alphabets,
where BM is typically applied.

A variation of this rule harkens back to the same idea as in KMP.
If U does not occur again in W,
then we can check what is the longest suffix of U
that is also a prefix of W,
and jump W further forward this way.


### General strategy

If both rules are implemented,
two jump tables must be calculated during preprocessing.
Then the sliding window is moved left to right, while comparisons are made right to left.
If a mismatch is found, the biggest jump between the two tables is made,
else W is found.


### Complexity

Preprocessing: O(m + |Σ|).

Search: O(n·m).
In other words, the worst case runs in the same order as the naive algorithm,
besides the cost of preprocessing.
A worst case example is S with repeats of a single character
and W matching all the way except its first character,
making jumps one character at a time,
and in fact with worse performance than the naive algorithm,
which scans left to right and will mismatch immediately.

Over all complexity: O(m + Σ + n·m) = O(Σ + n·m).
Σ is usually considered constant.
The search phase is the dominant term in the complexity analysis,
which makes sense, we don't want worst performance for preprocessing
than for the search itself.

In practice, this is rare.
Many improvements have been made over time.
If W is not complex, ie. non-periodic, we could arrive at 3·n (recent).

The lower bound is actually Ω(n/m),
which is the best complexity we could expect if we only preprocess W.
A simple example is W having different characters than S,
where we just make n/m jumps.

Compared to this algorithm's worst case of O(n·m),
Going back to KMP, the reason its complexity is so low
isn't so much the possibility of jumping forward,
as the fact that border positions aren't retested
since they are known to match.
Using the same idea for the second rule,
we may also achieve O(n + m).


### Raita's algorithm (1991)

This adds a simple heuristic to speed up eliminating non matches.
At a given window,
we check the first, last and middle characters of W in S,
and only proceed as usual if they match.

It can be useful for natural language processing,
where there are frequent common suffixes,
which would match all the time.


### Applications

BM is frequently used in language processing (natural, programming, etc).
It is hoped that a potential mismatch would appear early,
and if W is non-periodic, that is without many repeats,
we can make large jumps (see second rule).


## References
- http://monge.univ-mlv.fr/~lecroq/string: dozens of text searching algorithms with C code
- Cormen et al, Algorithms 3rd ed, chapter 32; see exercises
