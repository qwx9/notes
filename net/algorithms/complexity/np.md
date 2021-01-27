# NP-hardness

## Definition

Many common problems can be solved by exact algorithms,
but in at least exponential time.

In practice, often a non-optimal solution found in polynomial time is sufficient.
Typically, heuristics or approximation algorithms are used then.
The main difference between the two is that
heuristics alone do not provide a measure of error,
or distance to the optimal solution.
Approximation algorithms provide an upper worst-case bound for this distance.

A polynomial problem is termed _decision problem_,
and an NP-complete problem an _optimization problem_.

Given an NP-complete problem,
it is very improbable that a polynomial time solution exists.

Dynamic programming can solve both NP-complete
and polynomial-time problems,
but respectively necessarily in exponential time,
and polynomial-time.

Greedy algorithms are generally in polynomial time.
It is likely that a greedy algorithm exists for any problem,
but it won't always work.

Given an exercise with a known NP-complete problem,
and a proposed greedy algorithm,
we will probably be able to find a counter-example.


## Greedy algorithms

Uses local optima.
This is the case with Kruskal and Prim which both choose the minimum on each step
to provide a hopefully optimal global solution.
The choice of locally optimal solutions is termed *greedy choice property*.
A greedy algorithm reduces each problem into a smaller one by making one greedy choice after another.
Therefore it never reconsiders its choices, which is the main difference with dynamic programming,
which is exhaustive and guaranteed to find a solution.
Dynamic programming chooses according to all previous choices
and may reconsider the path to a solution.

A template of greedy algorithms is as follows:

	while SOL is not complete,
		x ← best element in remaining input
		SOL ← SOL ∪ x
		remove x from remaining input
	return SOL

Greedy algorithms are simple to express and implement,
but do not guarantee optimal results for many problems.
In the case of MST, both Prim and Kruskal can be proven
to arrive at a globally optimal solution by choosing locally optimal solutions.

A proof by induction (recurrence) works by proving a statement at step 1,
and showing that if it holds at step i, it will hold at step i+1 as well,
which proves it holds at any step.
This can be used in Prim's case.


## Heuristics

Heuristics are fast and simple, easy to understand and often empirical.
However, they do not provide any guarantee
on how far the solution is from the optimal one.


## Approximation algorithms

The goal is to arrive at the most near-optimal solution in polynomial time.
An approximation algorithm is said to provide an n-approximation
when the solution is n times bigger than the optimal solution.

Let I be an instance of an optimization problem with a large number of possible solutions.
Let c(I) be the cost returned by the approximation algorithm,
and c∗(I) the cost of the optimal solution.

In a minimisation problem, c∗ is the smallest possible value and c∗(I) ≤ c(I),
and we need c(I) / c∗(I) as small, ie. as close to 1 as possible.

In a maximisation problem, we need c∗(I) / c(I) as small, ie. as close to 1 as possible.

	an approximation algorithm for any given problem instance I
	has a ratio bound to ρ(n) where n is input size,
	if ∀n c(I) is within a factor of ρ(n) of c∗(I)

	minimisation: c(I)/c∗(I) ≤ ρ(n)
	maximisation: c∗(I)/c(I) ≤ ρ(n)

We can consider the worst-case performance of the algorithm versus an optimal one.
The notations ALGO, ALGO∗ for the worst values which form ρ are common.
