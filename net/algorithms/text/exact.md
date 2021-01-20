# Exact string search


## Definitions

	1-indexing
	A	alphabet
	a	element of A
	S	text sequence
	N=|S|	length of S
	W	word
	M=|W|	length of word
	S[i]	ith character
	S[i:j]	substring between i and j
	S[1..i]	prefix
	S[j..n]	suffix

Objective: find all occurances of W in S.

Complexity of algorithms involving preprocessing
is the sum of the complexity of preprocessing and that of the search.


## Naive algorithm

	i = 0
	while i < N - M
		j = 0
		while j < M and S[i+j] = W[j]
			j++
		if j = M
			output i
		i++

Complexity: O((N - M + 1) * M) = Θ(N * M)

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

Let w the base d equivalent of W[1..M]
and s the base d equivalent of S[t+1..t+M].
A match at t implies that s = w.

If we can compute w in Θ(M) time,
and all values s in Θ(N - M + 1),
we can then determine all the values of t by comparing w to each s
in Θ(M) + Θ(N - M + 1) = Θ(N).
We'll assume for now that the numbers are not exceedingly large.

	we note Θ(N - M + 1) since there are n - m + 1 different values for t,
	and asymptotically, if M = N, computing a single s value takes Θ(1) time, not Θ(0).

w can be computed in Θ(M) time using Horner's rule:

	w = W[M] + d * (W[M-1] + d * (W[M-2] + ... + d * (W[2] + d * W[1]) ... ))

s0 can be calculated in the same way from S[1..M] in Θ(M) time.
To calculate remaining values s1, s2, ..., sN-M in Θ(N - M) time,
we shall observe that st+1 can be calculated from st in constant time:

	st+1 = d * (st - d^(M-1) * S[t+1]) + S[t+M+1]
	
	subtracting 10^(M-1) * S[t+1] removes the highest order digit in s;
	multiplying by d shifts the number one position to the right;
	adding S[t+M+1] adds a new lower order digit.

	⇒ M = 5, st = 31415, d = 10, S[t+M+1] = 2:
	st+1 = 31415 - 10e5 * 3
		1415
	st+1 *= 10
		14150
	st+1 += 2
		14152

The constant d^(M-1) can be precalculated in O(log M) time,
but often a direct approach in O(M) suffices.

	⇒ See: Cormen et al, 31.6 powers of an element, pp 878

If the value is precalculated,
then the equation above takes constant time.
Thus, we can compute w in Θ(M) time and s0,s1,...,sN-M in Θ(N - M + 1) time.

An additional problem persists:
w and s can be too large for simple computation.
If W contains M characters,
we cannot assume that each arithmetic operation on w (M digits)
takes constant time.
Calling again upon techniques in number theory,
a simple solution is to calculate w and s modulo an appropriate integer q.

Observing that each of the w, si, and the si+1 equations
can all be computed modulo q,
w can be calculated modulo q in Θ(M) time,
and all s can be calculated modulo q in Θ(N-M+1) time.

Let q be a prime number such that d*q fits into a machine word.
All calculations can then be done in single precision arithmetic.

The recurrence formula is adjusted to be valid modulo q:

	st+1 = (d * (st - (d^(M-1) % q) * S[t+1]) + S[t+M+1]) % q

Let h = d^(M-1) mod q:

	st+1 = (d * (st - h * S[t+1]) + S[t+M+1]) % q

This solves the problem of dealing with large numbers,
but introduces the possibility of collisions:
while st ≠ w % q implies st ≠ w,
st ≡ w % q does not imply st = w.
We can therefore use st ≡ w % q as a fast heuristic test
to eliminate mismatches quickly.
For any t where st ≡ w % q,
we have to explicitely compare W and S[t+1..t+M+1].
With a big enough q, we can hope that this rarely occurs,
and that the cost of the full comparisons is low.


### Hashing and prime numbers

The step of computing w and s0 is essentially
computing a hash of W and S[t+1..t+M] using Horner's rule and modulo q.
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

Preprocessing: Θ(M)

Search: Θ((N - M + 1) * M) in the worst case,
but mean execution time, averaging some hypothesis, is better.

	if W = x^M and S = x^N, i.e. both have the same values everywhere,
	each position is a match and must be checked explicitely,
	which is the worst case.

Most of the time, we could expect that
only a constant number c of matches will be produced.
In that case, the expected time is O((N - M + 1) + c*M) = O(N + M).

We can do a heuristic analysis on the basis that
the reduction of values modulo q
works as a stochastic transformation
between the character set Σ* and Zq.
This is a difficult to formalize and prove problem (see Cormen 11.3.1).
We can then expect that the number of mismatches is O(N/q),
since the probability that some s is equivalent to p, modulo q,
is estimated at 1/q.

Let ν the number of matches.
Since there are O(N) mismatches and O(M) processing time for matches,
the total expected time for the Rabin-Karp algorithm is now:

	O(N) + O(M * (ν + N/q))

If ν = O(1) and if q ≥ M,
this time is O(N).
In other words, if the number of occurrences is low,
and that the chosen q is such that it is bigger than the pattern size,
we can hope that the algorithm takes only O(N + M) time,
and since M ≤ N, the expected time would be O(N).


### References
- Cormen et al, 11.3.1: using division for hashing,
wrt the discussion on expected complexity


### Applications

Generally, other algorithms with better performance like KMP and Boyer-Moore
are used to single pattern matching.

However, Rabin-Karp can easily be extended to multiple pattern searching,
where combined with the use of a set data structure like a bloom filter,
it is allegedly an algorithm of choice.
A search of k patterns would optimally result in O(N+k*M) expected time,
assuming the hash comparison is in constant time.
The Aho-Corasick algorithm has deterministically O(N+k*M) time.


### Knuth-Morris-Pratt

The KMP algorithm is actually a slightly improved Morris-Pratt algorithm,
but relies on the same idea.

Where the brute-force method must compare W and S[j] at every position j,
depending on where the mismatch occurs in W, we may restarting comparing
at an index in W higher than 0.

	If a mismatch occurs between W[i] and S[j+i] (with 0<i<m),
	then W[0..i-1] = S[j..j+i-1].
	Let U = W[0..i-1] = S[j..j+i-1],
	a = W[i], b = S[j+i], with a ≠ b.

		    j      j+i
	W	    [   U ][a] ...
	S	... [   U ][b] ...
	W´	      [ V ][c] ...

Let V be a suffix of U of length v.
It is possible that U is also a prefix of W.
In that case, instead of directly restarting comparison at W[0] and S[j+1],
we may instead restart at W[v] and S[j+v],
skipping this prefix we already know matches in both W and S.

We may thus precalculate a jump table T which for each position i in W
yields the length of the largest proper suffix of W[0..i-1] that is also a prefix of W,
corresponding to the index we may jump to, or -1 if there is no such suffix,
in which case we must resume at W[0] and S[j+1].
This proper suffix is also called a border (occurs at both ends of U).
A proper suffix of a string is not equal to the string and is non-empty.

	T[i] is the length of the longest border of W[0..i-1] for 0<i≤M.

Algorithm description from lecture:

	i = 0
	j = 0
	while j ≤ N - M
		if W[i] = S[j]
			i++
			j++
			if i = M
				output j - i
		else if i = 0
			j++
		else
			i = T[i]

This is wrong; if an occurrence is found, j must be reset using the jump table,
otherwise j is out of bounds on the next iteration.
Second, the loop must go up to N, otherwise a match at the end won't be
found (since i needs to go to N to compare the last characters).

	i = 0
	j = 0
	while j < N
		if W[i] = S[j]
			i++
			j++
			if i = M
				output j - i
				i = T[i]
		else
			i = T[i]
			if i < 0
				i++
				j++

### Preprocessing: constructing the jump table T:

We initially set W[0] = -1.
Since a mismatch at the very start of W is a special case,
where no backtracking is possible,
we denote this case with the value -1.
For i>0, if there is no border in W[0..i-1], we set T[i] = 0.

A naive algorithm can be O(M³).

We may use the same search algorithm to construct T in O(M) space and time,
comparing W with itself.

The insight is that at each position i,
one needs to consider checking suffixes of size x+1
only if a valid suffix of size x was found at T[i-1],
and should not bother to check x+2, x+3, etc.
(what?)

Note that the pseudocode on wikipedia doesn't produce expected results.

	T[0] = -1
	i = -1
	j = 0
	while j < M
		while i > -1 and W[i] ≠ W[j]
			i = T[i]
		i++
		j++
		if W[i] == W[j]
			T[j] = T[i]
		else
			T[j] = i

### Complexity

Complexity of search: O(2N - 1) = O(N)

We advance in S N times.
Any time there's a match with W[i], its position in S is remembered,
and rollback can occur at most i times.
Imagine the word is the same as S until the very end,
and that we have to roll back N times after N comparisons.
At most, we will get 2N - 1 comparisons.
Each character in S can be compared to W more than once,
but the number is bounded by M.


Complexity of preprocessing: O(2M - 1) = O(M)
Same explanation as above.


### Global performance

Complexity of the algorithm is O(N + M) time with O(M) space.
It is invariant regardless of how many repetitive elements there are in W or S.
There are dozens of variants of KMP.
Some have O(M*N) complexity at worse but work better than KMP in practice.
Each variant is designed for a particular application.

KMP has historic value but is not used much in practice.
In our case, preprocessing the text is much more effective than preprocessing the word,
hence the natural transition to sorted dictionary approaches.


## References
- http://monge.univ-mlv.fr/~lecroq/string: dozens of text searching algorithms with C code
- Cormen et al, Algorithms 3rd ed, chapter 32; see exercises
