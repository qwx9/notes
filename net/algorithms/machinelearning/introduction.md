# introduction to machine learning

## Definitions

### Data/Information/Knowledge

Elemental description of an object, observation, transaction, etc.

Information: way for an individual to know the individual's environment.

Knowledge: transition state of the data.

Together, these concept form the decision link (lien décisionnel),
which can be represented as a triangle:

	LIEN DECISIONNEL
		     D´ decision
	           ↗
	          …
		 K
	ECD	↗ ↖	BI business intelligence: visualization etc of data
	       D → I
	    DB databases
	    IS info systems
	    RIS relational IS
	machine learning: D→K …→ D´, decision

Data will have to be divided in at least two parts,
corresponding to the two-part process of machine learning:
design of the learning model (D→K),
and testing its genericity (K→D´).


## Preliminary steps, recommended practices

These steps are very recommended before any learning,
since the resulting model strongly depends on the nature,
distribution and quality of the input.


### Pretreatement (data engineering/data science)

Methods to clean up and format the data,
homogeneisation.
Objects can be of low quality: outliers or aberrant values,
which can be removed to avoid bias.

Objects must then be coded in a convenient manner,
ideally a vector, which allows for matrix representations of the data
as input to the model.
The data could also be seen as a graph, etc,
it just needs to be coded appropriately.


### Variable selection

Select descriptors/characteristics/etc to define the dimensions.

The first part is removing variables that have the same value everywhere,
since these are useless and just inflate complexity.

Then, strong correlations in the data must be detected,
to remove redundancy,
but will removal of one variable impact other correlated variables?
Metrics like pearson correlation is possible but done in a bidimensional space,
but here we have more than 2 dimensions,
so we must take into account multiple correlation,
and remove that to reduce redundancy.

Lastly, missing values are treated.
Ideally the data is complete, but most have missing values.
They can be imputed, or learning techniques that can handle missing value can be used.


### Normalization

Center (mean or center of gravity) and reduce data.
Centering moves data closer to a center of gravity
and helps the decision function in performing class separation.
Reduction scales all measures down to the same range/scale.


## Machine learning basics

Machine learning (or automated learning, etc) is discipline in artificial intelligence
consisting of modelizing a statistical (not mathematical) function,
to solve linearly or non-linearly separable problems.

	y = f(x)


### Problem domain

The data x is multidimensional,
and there are multiple x.

	x ∈ ℝ^p, where p = number of dimensions (p-dimensional space).
	x = xᵢ, i = {1,... , n}

On the other hand, y can be mono- or multi-label,
ie. it can be a vector, a single column,
representing the variable to explain,
with zB binary classification, biclass decision problem).
However there can be more than two values/classes,
transforming to a multiclass problem,
where the model must target multiple explanained variables simultaneously,
zB targeting multiple pathologies (predicting multiple pathologies) from genomic data,
in which case y the target is a matrix,
which represents the label space.

In addition, ideally all x are represented by p variables.
However, x can be represented by heterogenous variable spaces = multi-views.
For instance, some x ∈ ℝ^m and some x ∈ R^s where m ≠ s.
In this case, multi-view learning takes into account spaces which do not intermix.

All of this defines the application domain,
based on the constraints on the input data.
Targets or explained variables can be constructed from discrete terms,
in which case it's a classification problem.
Numeric score would be a regression problem.


### Statistical functions

In 2D, linear separation between two classes is a straight line.
In 3+D, it would be a hyperplane.

Suppose now that the curve we draw to perfectly separate two classes in 2D
is not a straight line, resembling a polynome,
which we can't really define exactly.
This is why we talk of statistical functions, not mathematical ones:
we cannot express them exactly.
We therefore approximate the function,
which we denote by a ~.

	y = f̃(x)

Approximation implies there being an error term,
and a minimization problem.

	minimize ||f - f̃|| = ε

Machine learning builds f̃ based on the data,
which underlines the importance of data quality.


### Types of learning

			LEARNING
	SYMBOLIC				NUMERIC
	discrete maths			applied maths, statistics
				DETERMINISTIC			GENERATIVE (probabilistic)
				single class attributed		classify statistic with
				to each statistic		different probabilities for every class
		SUPERVISED	UNSUPERVISED	SEMISUPERVISED
	y completely		y completely	y partially known
	known			unknown		(most practical cases)

In practice, semisupervised learning corresponds
to partial labeling of the data by experts,
where labeling the entire data is too costly.
The problem there is finding a compromise between the geometric structure
induced in the non-supervised part
and the little supervision driven in the supervised part.

				SUPERVISED LEARNING
	DISTANCE-BASED		CONNECTIONIST	KERNEL		INDUCTION	GRAPHICAL	REGRESSION
	→ KNN: k-nearest	→ NN		(noyaux)	→ DT, RF	→ BN		→ min cost
	  neighbours		→ deep NN	→ SVN		(one or more	(reseaux       of function,
	  							trees)		bayesiens)    linear or not
Here we'll focus on connectionist approches,
starting with simple networks with a single neuron.


## Neural network supervised learning

### The formal neuron

A treatment unit which receives an input vector X
and returning a real output S based on a vector of connection weights W
of the same dimensions as the input data.

	X = (x₁, ..., x₏)
	W = (w₁, ..., w₏)

To arrive at the decision S, there are two successive transformations.
The first F is linear and uses X and W to calculate its activation a.
The second f is non-linear and calculates S.

	a = F(X,W)
	S = f(a)

f can be a threshold function, or sigmoid, identity, hyperbolic function, etc.

	x₁   ­→
	x₂   ­→
	x₃   ­→     < F(X,W) → a → f(a) >    →    S
	…    ­→
	x₏   ­→

There are two types of formal neurons.
The scalar/dot product neuron produces F from the dot product of X and W.

	F(X,W) = <X,W> = ∑ᵢ₌₁ⁿ wᵢxᵢ

The distance neuron is still defined by F(X,W)
but is obtained by a norm between the two vectors X and W.
This corresponds in a way to the euclidean distance between the two vectors.

	F(X,W) = ⏸X - W⏸²₂

If the weights are normalized, a link can be made between the two neuron types.

	⏸X - W⏸²₂ = ⏸X⏸² + ⏸W⏸² - 2<X,W>
	↑distance			↑dotproduct
	⏸X⏸² > 0, ⏸W⏸² > 0
	if W normalized to X, 2<X,W> > 0

When 2<X,W> is very large, the distance plummets, and vice-versa.
Small distance means the neuron (W) is close to the data (X),
which implies that X contributes largely to activation,
meaning the dot product is big.
Same for large distance.


### ADALINE: Adaptive linear element

#### Definition

Input data is supposed to be in n-space,
that is each xᵢ is a vector of n dimensions.
Since we are doing supervised learning,
to each vector is associated expected output D.

	X = x¹, … , xⁿ
	D = d¹, … , dⁿ

xᵢ are termed _variables_.
Each sample is n-dimensional with one value for each variable.

ADALINE is a linear, adaptive, 2-class model,
which computes an output y based on connection weights W,
such that:

	y = w₀ + <W,X>
	= straight line

w₀ is the bias unit,
without which y would always go through the origin 0,0,
which reduces the search range.
It is added to extend the search to the entire search space.

	"élargir le spectre de recherche de solutions de séparation
	entre les données d'une classe et les données d'une autre."

Mathematically, the equation of the curve representing the ADALINE
is of the form:

	w₀ + w₁x₁ + … + w₏x₏ = 0

ie.:

	w₀*1 + w₁x₁ + … + w₏x₏ = 0

If we omit w₀, the curve will always pass through the origin, which
does not allow us to explore the full problem space.
w₀ allows the curve to be offset anywhere in addition to changing its slope.


#### Algorithm

We want a stable and interpretable method with minimum error.

	"initialisation → modèle bête,
	puis rendre le modèle intelligent/intélligible/interprétable
	pour le client non-expert"

1. at t=0, W is initialized arbitrarily/randomly
2. present a data x^k and output d^k
3. compute distance e between output and expected output

	e = d^k - <W,x^k>
	dotproduct result is scalar, d^k must also be scalar
	therefore e is scalar

4. compute approximation Δ

	Δ = -2ex^k
	x^k is a vector, therefore Δ is a vector

5. recompute weights

	w(t₊₁) = w(t) - ε
	ε: learning step

6. repeat 2.-5. until stability (convergence):
for each xᵢ, and over multiple passes over the db


#### Example: learn logical OR using ADALINE

Let D a 2D data matrix with rows for `samples' and columns for `variables'.
Logical OR is a binary operator, hence 2 variables and 2² = 4 possible values,
therefore: 4x2 matrix.
Let's assume true=1 and false=-1.
Let d be the vector of desired output.

	#	V₁	V₂	d
	1	1	1	1
	2	1	-1	1
	3	-1	1	1
	4	-1	-1	-1

We have thus modelized the problem as a learning dataset.
We could visualize this as a simplistic scatterplot:

        ⊗   1┼    ⊗
	     │
	┼────┼────┼
	-1   │    1
	⊙  -1┼    ⊗

	⊗ class 1
	⊙ class -1

By initializing the weights arbitrarily,
we essentially draw an arbitrary line:

![After initialization](introduction.001.png)

Then, at each iteration,
the algorithm will compute approximation distance and adapt weights,
and the curve will move in 2D space until convergence.

![At convergence: green curve](introduction.001.png)

It is these final weights that are finally accepted
to test the model, ie. if new data is well separated.


#### Exercises

##### ADALINE

We wish to learn logical AND.
Modelize the problem for learning it using ADALINE
with a step ε = 0.1,
and initial weights (w₀, w₁, w₂) = (0.3, 0.8, 0.4).
Unroll one iteration.

	Assume true=1, false=-1.
	W is of length 3, so we have 3 corresponding variables.
	V₁ and V₂ are the possible values on each side of the AND operator.
	V₀ then must be always 1 so as to not influence the desired result d.
	w₀ is the bias term.

	Therefore, let D be the following matrix:

	#	V₀	V₁	V₂	d
	1	1	1	1	1
	2	1	1	-1	-1
	3	1	-1	1	-1
	4	1	-1	-1	-1

	We now proceed to learning starting with the first sample:
	X = (1 1 1), d = 1.
	e	= d - <X,W>
		= 1 - (0.3 0.8 0.4) [1\ 1\ 1]
		= -0.5
	Δ	= -2 * (-0.5) (1 1 1)
		= (1 1 1)
	w(1)	= w(0) - ε * Δ
		= (0.3 0.8 0.4) - 0.1 * (1 1 1)
		= (0.2 0.7 0.3)

	Iteration 2:
	X = (1 1 -1), d = -1
	We calculate e, Δ, then use w(1) to compute w(2).
	And so on.

	We repeat with the remaining samples.
	When we reach w(4), we wrap around and start over with the first sample.
	This repeats until w(t) stabilizes.
	We will obtain in the end:
	w∗ = (-0.5 0.4286 0.3571)

	Visually, we can see that the final curve separates the samples well.
	We need a computational method to verify this.
	We'll take each of the samples and test them one by one,
	by multiplying them by the final w.
	If the result is ≥ 0, it's in class 1, and -1 otherwise.

	1: (1 1 1) [-0.5\ 0.4286\ 0.3571] > 0	⇒ s = 1.
	s = d ⇒ ok.
	2: (1 1 -1) [-0.5\ 0.4286\ 0.3571] < 0	⇒ s = -1.
	s = d ⇒ ok.
	etc.
