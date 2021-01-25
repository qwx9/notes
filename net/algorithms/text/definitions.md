# Definitions

## Nomenclature

	1-indexing
	Σ,A	alphabet
	Σ∗	language: set of possible words ∈ Σ (∗: repetition, like regex *)
	S	text sequence ∈ A
	n=|S|	length of S
	P,W	word
	m=|P|	length of word
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
