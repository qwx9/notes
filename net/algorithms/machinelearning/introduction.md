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
