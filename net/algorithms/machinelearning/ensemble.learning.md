# Ensemble learning (fr. apprentissage ensembliste)

## Introduction

### wrt supervised learning

Supervised learning produces a set of rules
from a labeled training database,
in order to predict the classes of new data.

However, they produce errors,
because of 3 things.


#### Noise

Noise is created by erroneous labeling,
which can be estimated.


#### Variance

Variance, eg. algorithm's statistical limits
is the part of the error attributed to the data's specificities.

For instance, there the problem space may be so big
as to have multiple possible separating hyperplanes,
which would for instance generate different entropy-based DTs,
which are all capable of separating the data.
The algorithm is forced to choose, and may not make the optimal choice.
This means it will work for some but not all datasets.

In that case, instead of choosing one solution,
we could use all of them.
This is called a _vote_,
and reduces this error.


#### Bias

This is due to the algorithm's inability to adapt to the data.
For instance we choose to use a linear separator for non-linearly distributed data.
The algorithm will then fail to separate the data,
and will once again produce multiple solutions
that misclassify individuals differently.

Once again, we may combine all the solutions
to produce an adaptable and better model.


### Definition

The last two types of error can be reduced by so-called ensemble methods.
These combine the decisions of individual hypotheses to classify new data,
based on the "union makes strength" principle.

There are 2 conditions for this to work.
The constructed hypotheses must have a better success rate
than random ones,
ie. error less than 0.5 for two balanced classes.
They must also have a certain diversity.


### Theory

If multiple hypotheses are to work together,
they must be good and also different.
Regardless of error, two models that are the same
will produce the same result as one.

Let T be a set of *independent* classifiers
which each commit an error E_gen = ε.
In other words:

	E_all = ∑k=N/2^N C_k^N·ε^k·(1-ε)^(N-k)

![Error distribution based on individual error magnitude](ensemble.learning.001.png)

If the final decision is by majority,
at least 50% of the decisions must be in error.
However, in theory, this is never verified.
When multiple classifiers are all trained on the same data,
they are necessarily dependent,
even if none of them depend on another's decision.


### Types

There are two types of methods for ensemble classifier construction.
The final hypothesis can be by complete vote or vote by majority, etc.


#### Heterogeneous

This combines a set of hypotheses produced by different algorithms
(of any type),
or simply different parameters/initializations for the same algorithms.
For instance DT with and without pruning,
neural nets with different layer number and sizes, etc.

All of these train on the same distribution of the data.


#### Homogeneous

Here we use the same algorithms with the same parameters,
but using different data,
formally the distribution of the individuals' parameters.
This means providing different samples each time.

The algorithms consider that individuals are of equal weight.
Here, the distribution of probabilities (weights) are changed each time,
eg. different distributions of the individuals,
all from the same initial data.

In other words, from the initial dataset,
we generate many different new datasets.
Distributions may be built by random sampling,
or attributing weights differently each time.


### Building homogeneous models

There are two construction strategies,
iterative adaptive techniques ("boosting"),
and parallel random techniques ("bagging").

Choosing between the two depends on the input data's structure.
Bagging would work better on a dataset with a huge function space,
while boosting would work better on a dataset
where the classifiers can't manage to adapt to the data
because it's too complex.


#### Boosting (fr. dopage)

Here an adaptive strategy is use to boost performance.
It is applicable to any kind of algorithm.

Basically, the first model will commit some classification errors.
The following one will concentrate on misclassified individuals.

Boosting is a general method for converting weak prediction rules
into (much) stronger ones.

Formally, given a "weak" algorithm L
which can always return a hypothesis h
with an error rate of ≤ 0.5 - γ,
where γ is the performance gain of h compared to random.
Therefore, a boosting algorithm can be proven
to produce a decision rule (hypothesis)
of error rate ≤ e.

Essentially boosting reduces bias.


##### Example: horse racing

Experts here will never produce a simple betting rule that always works,
ie. accuracy will never be 100%.
However, in some cases, they may produce better than random rules.

Individually weak rules include
betting on the horse that has had the most wins recently,
or the one that has the highest number of bets,
or one that prefers difficult terrain.
These all may work better in specific instances.

The idea then is to ask the expert for a heuristic (rule).
We get the number of successes out of 100 races.
Missed races are called "difficult cases".
We then ask the expert to look at only the difficult cases
and produce another rule,
and so on.
In the end, all the heuristics are combined.


##### Adaboost, 1996 Freund and Schapire

This is an iterative algorithm for adding classifiers,
with variants of the training data
obtained by iteratively attributing weights
on the same data,
ie. weights are computed to focus on misclassified,
difficult cases, from the previous iteration.

Say we train a weak learning algorithm on a two-class labeled database.
An example of a weak algorithm is DTs because of overfitting.
Initially all individuals have the same weights.
On each iteration, we'll learn a classification hypothesis
on the data but using a distribution based on the weights.
DT construction has to be modified to take weights into account.
We then ask the classification algorithm to predict the training dataset.
For this, the classifier must not learn the data by heart,
if we use DTs, we have to do pruning.
The error will be the difference between predicted and expected
(rightly classified or not)
by individual,
multiplied by the weights.
In the balanced scenario, if the error is over 0.5,
then we stop since we are worse than random,
and we return the previous classifier.
Otherwise, we calculate a predictive capacity metric
which tends towards infinity when the error tends towards 0.
We then compute the new weights
using the current weights and a conditional value for that metric
based on whether the individual was misclassified or not.
This way misclassified individuals see their weights increased,
and well classificed ones see their weights decrease.
The final decision would be essentially the sum of
the product of the individual hypotheses
multiplied by their predictive capacity.
A classifier that didn't have a high error
will have a large predictive capacity,
hence a larger influence on the final decision.

![Dataset](ensemble.learning.002.png)
![Step 1](ensemble.learning.003.png)
![Step 2](ensemble.learning.004.png)
![Step 3](ensemble.learning.005.png)
![Final classifier](ensemble.learning.006.png)

Here the result is the sign of the sum,
class being either 1 or -1.

Boosting works well,
but there are even better approaches based on the same idea.
Its power stems from the adaptive resampling.
It reduces variance a little,
but mostly it reduces bias,
providing a flexible classifier.
Convergence is fast.
However, it is very sensitive to noise,
as it will overadjust over the bad examples.


#### Bagging

Here classifiers train on random samples
and their decisions are independent.
Samples can reuse individuals,
which is termed random sampling with replacement.
(fr. échantillonnage avec remise).
Such sampling strategies are named "bootstrap",
hence the name bagging: bootstrap aggregation.
In other words, in each new bootstrapped dataset,
some individuals will be duplicated
and others missing,
which adds some randomness in the process.
The same learning algorithm is applied to each of these,
and the final prediction is obtained by vote.

Bootstrap greatly increases the performance of learning algorithms
such as DTs.
Plotting error rate of DTs with bagging against those without bagging
shows that it is greatly reduced when bagging.

Bagging does not reduce bias.
Unlike boosting, this has a random strategy, not an adaptive one.
Each hypothesis is built on a different dataset,
thus attenuating variance.
Given that it's an ensemble method,
the risk of overfitting is practically zero,
by averaging classifiers built with different random samples of the same data.

In addition, bootstrapping helps stability of an algorithm's processing.
Discriminatory methods are said to be unstable
if a small change in the data provokes big changes in the model,
for instance with neural nets or trees.
This is very important for applications in medicine for instance,
since an unstable algorithm depends on the data.
The problem is compounded with variable selection.


#### Random forests, 2001 Breiman

The idea here is create a better classifier
from a set of weak classifiers, ie. DTs.
For this, the trees must complement each other,
ie. they have different results and must complete each other.
The more the trees are independent and efficient,
the better the result.

The approach consists of building a large number of random trees
for the same problem,
ie. a _random forest_.

Randomness is added before and during tree buildcing,
then the results are aggregated by accepting votes by majority.
Essentially, RF combines random selection
of both instances and of variables.
The first step is to perform bootstrapping.
Then, CART trees are built,
and instead of using a gain function to decide which variable to select,
a subset of variables is selected at random,
then the one with the best gain among those.
The decision trees are complete,
ie. built automatically without any pruning.
Bad variables are simply compensated by others.

Hyperparameters of the model here are
the number of variables to choose at each step,
and the number of trees.

Pros of this method is the bootstrapping.
Essentially 2/3 of the examples in each sample
are unique in the dataset, and 1/3 are duplicates.
Therefore for each subset 1/3 of the examples in the dataset
are not selected and termed _out of bag (oob)_,
and will be used for internal validation of the forest
(estimation of the classification error in generalizing the forest),
and to estimate the importance of the variables
for their selection.

This way, without splitting the data or cross-validation,
unbiased validation can be performed on the entire dataset,
since we aren't really testing on the same dataset.

For each individual, we select all the trees in which it was oob,
meaning trees which didn't use this individual for training.
We can then ask these trees to predict the individual's class.
Then, the final prediction based on all of the trees
is compared to the expected result.
This gives an error estimate on the oob.
Breiman argues that this estimate is very close
to the one that would be obtained if the dataset
was validated using splitting or cross-validation.

Since RFs are an ensemble method,
algorithms are stable and uses multiple classifiers for variable importance.
A variable is considered important if its modification entails misclassification.
However, we can't decide this based on one individual or one classifier.
Based on the constructed trees,
we can deduce a hierarchy of the variables,
hence enable variable selection.
For every tree, the oob's classes are predicted.
Next, a given variable is selected
and its value is permuted randomly for the oob's.
They are predicted again with this new value,
and the predictions are compared via metrics like before.
Then this is averaged over every tree,
and can then be done for other variables.


##### RF optimisation

For the error to be low,
the trees must be complementary,
ie. good and uncorrelated.
We must then find a compromise between intercorrelation
and prediction performance ("force").
What influences both is the number of variables to select hyperparameter.

With fewer variables, there's a bigger risk of selecting bad variables
(lower chance of selecting the best),
and the bigger the difference between trees.
However, force will suffer since we often choose bad variables.
On the other hand, more variables means more intercorrelation but bigger force.

Usually, we just try different values of the parameter,
but there is research to support always choosing
the square root of the number of variables.

The number of trees can be raised without risk of overfitting.
The error rate of generalization coverges towards a limit value.
