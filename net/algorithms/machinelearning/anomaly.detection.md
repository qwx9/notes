# Anomaly detection

## Definition

The goal here is to detect individuals
which have non-conforming behavior.
Applications include network intrusions,
credit fraud detection,
surveillance, etc.

Removing or modifying atypical observations from a model
without justification is wrong and unwarranted.
The idea is to identify them,
since they are the most likely to be a result of a measuring error
(to be confirmed),
a label error,
or an anomaly.

If anomalies is detected in data for supervised learning problem for instance,
it could be justified to remove them to improve the classifier,
as outliers.


## Supervised approach

Here we know that there are anomalies
and non-anomalies in the data,
but with the assumption that anomalies are rare.
We consider that ≤1% individuals are abnormal.
The problem then is that the data is imbalanced.

If we choose a classifier which always calls normal,
we'd have a classifier with 99% accuracy,
which is usually great,
but here it depends on the context:
do we have 99% the same predicted class or not.
If not, it is of interest.

In addition to needing the anomalies to be pre-labeled,
with imbalanced data
typical supervised approaches
such as DTs, RFs, MLP etc.
will all have a problem.
It is then necessary to adapt these algorithms.
One example is imbalanced classification RFs.
These often do rebalancing,
such as boosting the algorithm to focus on anomalies.
Techniques include sub- and over-sampling.

Subsampling means removing the anomalous data,
sampling normal data of the same size as the abnormal data,
then a classifier is built on that,
and this is repeated in order to reduce the normal samples.
We would get lots of classifiers built
with the same size between normal and abnormal
and capable of differentiating between them.
However, the final classifier was built in an environment
where normals have the same size as the abnormals,
whereas this isn't true for the data.
Therefore the probabilities for both classes must be rebalanced
so they are brought down to the same original probability space.


## Unsupervised approach

Here there are no labels,
but we suspect that anomalies exist in the data.
There again we suppose abnormal data to be very rare.


### k-nearest neighbors

Here an anomaly or isolated observation
is qualified by its proximity to neighboring points.


### One-class SVM

Here observations are all separated
from the origin in the representation space,
to maximize the margin,
ie. the distance between hyperplane and origin.
We create the densest possible sphere
containing the individuals.
Those distant from this hyperplane,
that are unable to be contained within it,
are anomalies.


### Isolation forest

This is currently one of the best techniques,
and are not based on DTs like RFs.

The idea is that a rare individual
is more isolated than others.

![Isolation example](anomaly.detection.001.png)

In the example,
we are in 2D space,
and x₀ is an anomaly which we want to isolate
by placing it in its own square.
With random partitioning,
a normal observation would take many iterations to be separated
since it is very close to the others,
while an anomalous one would be relatively quick,
since it is relatively different.

An isolation forest is therefore
a large set of trees.
One random variable is selected, for instance age,
and we choose a random threshold to separate the data
in two classes (for instance <47 years).
Then we do the same with other variables.
In the end, every individual is a single leaf.
The more "questions" that must be asked
for an individual to be separated
(eg. the path is long),
the less likely that it is an anomaly.
Many trees are generated this way,
and the final score for any individual
is the average of the isolation.
