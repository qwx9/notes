# Unsupervised machine learning

## Definition

It consists of inferring knowledge on date
based on only the training data.
There are no targets, we look for new structures in the data,
ie. clusters of individuals:
how different are they between each other?

This is often the case where there are insufficient funds,
or time, or experts.

There are two major steps:
dimensionality reduction and clustering
(sedimentation/grouping),
ie. automatic construction of classes.


## Dimensionality reduction

Suppose the variable space is p-dimensional.
Each variable corresponds to an axis in one dimension,
but if we try to visualize the distribution of the individuals,
we cannot use more than 3.

The objective is here to represent the observations
in a space of reduced dimensionality
with the smallest possible loss of information.


### Principal components analysis

Say the data is student marks in different subjects.
Intuitively, we may choose the mean as one axis,
since it seems that most marks would be correlated.

PCA reduces the number of dimensions
by considering the correlations between variables.
Finally it creates new uncorrelated axes.
In other words,
PCA analyses correlations between variables
in order to create new uncorrelated variables
with high variance,
ie. high information content
(fr. fort pouvoir informatif),
by linear combinations of the initial variables.

The new variables are names principal components,
which determine principle axes,
and are ordered by importance (variance).
The totality of the principal components
contain all the initial information,
but the aim is to only choose a small number
with a large fraction of it.

The important questions then are
how to choose the number of axes,
and how to interpret them.


### Clustering

The goal here is to partition a set of individuals
into multiple subsets that are as homogeneous as possible.
There are two main methods:
partitioning (k-means) and hierarchical (CAH).

We don't know the number, shape or size of the clusters.
The methods will attempt to group everything that is similar,
distance everything that is different.
The key is the similarity criteria (distance function).

In other words, we want to minimize intra-class inertia
and maximize inter-class intertia.
_Intra-class_ inertia is defined
as the sum of the distances to the center of gravity
of the individuals within the cluster.
_Inter-cluster_ is the sum of the distances between centers of gravity
and the center of gravity of the total population.
The sum of distances between each individual
and the center of gravity of the population is named _total inertia_
and is equal to the sum of inter- and intra-class inertias.

Since total inertia is a constant,
if we minimize one we maximize the other.
The best clustering is each individual as its own cluster.
We want an optimal compromise.


#### K-means

Here, we are obligated to choose a number of clusters.
We choose then k points at random,
and affect each point to the nearest center.
Then we recalculate centers of gravity for each partition,
which is why this technique is also referred to as "moving centers".
We then attribute a partition to each individual again.
This is done until stability.


#### CAH, Hierarchical clustering

Here a distance matrix between individuals is calculated.
Based on these pairwise distances,
the two closest individuals are chosen.
Then the distance between this couple
and the other individuals is calculated.
This necessitates a distance aggregation criterion,
such as average, min, max, etc.,
used to calculate distances between couples and the rest.
We then choose again the lowest distance,
and repeat until there is only one cluster.

By choosing a height to "cut" the resulting dendrogram at,
we get the number of clusters.
