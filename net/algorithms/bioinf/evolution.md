# Text algorithms and evolution

## Homology and similarity

Text searching algorithms are applied to look for similarities
between sequences to detect homology.
Two sequences are homologues if they share a common ancestor.
Homology can occur at multiple levels,
independent of each other: genomes, chromosomes, genes, nucleotides.
Similarity is quantitative, homology is qualitative.

We assume a correlation between similarity and homology.
However, two sequences can be homologues but dissimilar and vice versa,
from evolutionary convergence, low complexity, or a statistical threshold problem.

Motivational examples:

1. 2 sequences of length 20, 7 identical nucleotides.

	if non-homologue,
	probability of having identical nucleotides on 1 site = 1/4 (alphabet size = 4)
	⇒ E(identical sites) = 20*1/4 = 5
	⇒ binomial distribution B(20,1/4): p(K≥7) = ?
		p(K≥7) = Σk p(K=k) ∀ k∈[7;20]
			p(K=k) = dbinom(k, 20, 1/4)		
			p(K≥7) = sum(sapply(7:20, function(x) dbinom(x, 20, 1/4)))
		p(K≥7) ≅ 0.21
	⇒ we cannot deduce homology or lack thereof

2. 2 sequences of length 20, low complexity (AT repeat), 20 identities

	⇒ probability of 100% similarity due to chance very low
	but, we cannot conclude that there is homology
	low complexity repeats are very common
	intuitive definition of low complexity: can compress sequence to N * pattern

3. 2 sequences of length 20, 19/20 similarity

	⇒ probability of 19/20 similarity due to chance < 10e-10
	and yet, in a 3Gbp genome,
	the probability of finding a sequence by chance is now ≅ 0.2
	so we still cannot deduce homology.

4. 2 sequences of length 50, 49/50 similarity

	very unlikely (10e-16) to find by chance
	a 50bp sequence with 49/50 similarity in 1000 3Gb genomes
	and yet, we must still take complexity and genome composition into account.
