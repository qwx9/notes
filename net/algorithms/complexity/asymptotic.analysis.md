# Complexity analysis

## Rate of growth

Rate at which running time increases as a function of input.
Approximate by ignoring low order terms
that are relatively insignificant
for a large value of input size (n).

	⇒ n⁴ + 2n² + 100n + 500 ≅ n⁴

Commonly used rates of growth:

	1 < log logn < √logn < log²n < 2^(logn) < n < log(n!) < nlogn < n² < 2ⁿ < 4ⁿ < n! < 2^(2ⁿ)

1	constant
logn	logarithmic
n	linear
nlogn	linear logarithmic
n²	quadratic
n³	cubic
2ⁿ	exponential


## Asymptotic analysis

	worst case (lower bound) ≤ average case (average time) ≤ best case (upper bound)

Let f(n) be the function representing the algorithm.


### Big-O notation: upper bound

Generally represented as f(n) = O(g(n)),
where g(n) is the upper bound of f(n) == maximum rate of growth.
g(n) is an asymptotic tight upper bound for f(n).

Formally:

	f(n) = O(g(n)) if 0 ≤ f(n) ≤ k·g(n) ∀ k > 0 and ∀ n ≥ n₀
	i.e. f(n) = O(g(n)), n → ∞
	i.e. f(n) growth rate ≤ g(n) growth rate

	⇒ f(n) = n⁴ + 2n² + 100n + 500		⇒ g(n) = n⁴
	⇒ f(n) = 3n + 8				⇒ g(n) = n
	⇒ f(n) = n² + 1				⇒ g(n) = n²
	⇒ f(n) = 431				⇒ g(n) = 1

Generally, we only focus on the upper bound.
The lower bound is of no importance,
and Θ notation is used if both upper and lower bound are the same.


### Omega-Q notation: lower bound

Generally represented as f(n) = Ω(g(n).
At larger values of n, the tigher lower bound of f(n) is g(n),
that is the largest rate of growth which is less than or equal
to the given algorithm's rate of growth f(n).

	⇒ f(n) = 100n² + 10n + 50		⇒ g(n) = n²


### Theta-Θ notation: order function

If the upper and lower bounds of f(n) are the same,
then so is the average rate of growth Θ(g(n)).
Otherwise, all possible time complexities must be averaged.

Example: quicksort.


## General rules for asymptotic analysis

- loops: at most: running time of the statements * number of iterations
- nested loops: product of all sizes of all the loops

	⇒ analyze from the inside out!

- consecutive statements: sum of time complexities of each
- conditionals: worst-case running time, the larger of the possible blocks
- logarithmic complexity: O(logn) ⇒ takes a constant time to cut a problem size by a fraction (usually ½)

	⇒ for(i=1; i<=n;) i=i*2;		or decreasing: for(i=n; i>=1;) i=i/2;
		k iterations: at kth step:
		2^k = n
		log 2^k = logn
		assuming log2:
		k·log2 = logn
		and:
		k = logn
		⇒ total time is O(logn)

	⇒ Another example: binary search


## Properties

- transitivity: f(n) = O(g(n)) and g(n) = O(h(n))	⇒ f(n) = O(h(n))
- reflexivity: f(n) = O(f(n))
- if f(n) = O(kg(n)) ∀k>0, then f(n) = O(g(n))
- if f1(n) = O(g1(n)) and f2(n) = O(g2(n)), then (f1+f2)(n) = O(max(g1(n)), g2(n)))
- if f1(n) = O(g1(n)) and f2(n) = O(g2(n)), then f1(n)f2(n) = O(g1(n)g2(n))
- O(g1(n) + g2(n)) = O(g1(n)) + O(g2(n))
- O(g1(n) * g2(n)) = O(g1(n)) * O(g2(n))


## Logarithms

	log x^y = y log x
	log xy = log x + log y
	a^log_b^x = x^log_b^a
	log n = log_10ⁿ
	log^k n = (log n)^k
	log x/y = log x - log y
	log_b^x = (log_a^x) / (log_a^b)
	Σ_kⁿ logk ≅ n·logn
	Σ_kⁿ k^p = 1^p + 2^p + ... n^p ≅ 1/(p+1) * n^(p+1)


## Series

- Arithmetic:

	Σ_kⁿ k = 1 + 2 + ... + n = n·(n+1) / 2

- Geometric:

	Σ_kⁿ x^k = 1 + x + x² + ... + xⁿ = (x^(x+1) - 1) / (x - 1), x ≠ 1

- Harmonic:

	Σ_kⁿ 1/k = 1 + 1/2 + ... + 1/n ≅ log n


## Divide and conquer master theorem

Divide and conquer algorithms divide the problem into subproblems,
then perform some additional work to compute the final answer.

Merge sort operates on 2 subproblems, each half the size of the original,
then performs O(n) additional work for merging.
Thus its running time equation is:

	T(n) = 2·T(n/2) + O(n), 
	where 2·T(n/2) is the 2 subproblems of half size

Running time of divide and conquer algorithms
can be determined without fully solving them,
if the recurrence is of the form:

	T(n) = a·T(n/b) + Θ(n^k·log^p n),
		where a≥1, b>1, k≥0 (constants) and p is a real number.

- if a > b^k 		⇒ Θ(n^log_b^a)
- if a = b^k:

	if p > -1 	⇒ Θ(n^log_b^a·log^(p+1) n)
	if p = -1	⇒ Θ(n^log_b^a·log log n)
	if p < -1	⇒ Θ(n^log_b^a)

- if a < b^k:

	if p ≥ 0	⇒ Θ(n^k·log^p n)
	if p < 0	⇒ Θ(n^k)

	⇒ a = 2ⁿ: not applicable, a is not constant
	⇒ a·T(n/b) - Θ(n^k·log^p n)): not applicable, negative function

Such a theorem exists of substract and conquer recurrences as well.


## Induction method: guessing and confirming

This can be used to solve any recurrence
by guessing the answer and proving it correct by induction.

[...]


## Amortized analysis

Determination of time-averaged running time for a sequence of operations.
Unlike average case analysis,
amortized analysis does not make any assumption
about the distribution of the data values,
where average case assumes data are not "bad".
Amortized analysis is a worst-case analysis
for a sequence of operations rather than individual ones.

Worst case analysis can provide an overly pessimistic bound.
Amortized analysis generally applies to a method that consists of a sequence of operations,
where the vast majority of the operations are cheap,
but some of the operations are expensive.
If we can show that the expensive operations are particularly rare,
we can change them to the cheap operations, and only bound the cheap operations.

When one event in a sequence affects the cost of later events,
one particular task may be expensive,
but it may leave data structures in a state that the next few operations become easier.

Example: array of elements from which we want to find the kth smallest element.
We can first sort the array and return the kth element from it.
Assuming the cost of performing the sort is O(nlogn),
if we perform n selections, then the average cost of each selection is O(nlogn/n) = O(logn).
This indicates that sorting once is reducing the complexity of subsequent operations.


## Problems
- examples of program analysis in Karumanchi,
Data Structures and Algorithms Made Easy, section 1.28


## References
- Karumanchi, Data Structures and Algorithms Made Easy: 1.10-1.28
