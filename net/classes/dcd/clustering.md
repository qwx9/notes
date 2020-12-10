# preference-based pattern mining

- clustering_overview.pdf


## definition

- on ne cherche pas uniquement un sousensemble de tuples
pour definir un sousgroupe d'interet,
mais de construire une partition dans les donnees,
et d'affecter les objets a une partition
- on veut une partition tq chaque partition contient des obj similaires,
et les obj entre partitions differentes sont dissimilaires
- on s'appuie donc sur des mesures de distance/similarite

[](/classes/dcd/clustering.001.png)

- formellement, partitions tq les similarites intraclust sont maximises,
et les similarites interclust minimisees
⇒ probleme d'optimisation
- etant donne un ensemble o_m d'obj definis par un ensemble a_n d'attributs,
on veut grace a une mesure de similarite,
affecter un cluster a chaque objet

- on est ici dans un cadre non-supervise,
on ne connait les identifiants des clusters a l'avance
	* zB entrainer un modele sur des patients,
	2 classes sain et malade claires et predefinies
	* ici on n'a pas d'attributs de classes,
	c'est a l'algo de le trouver

- on a plusieurs variantes a ce probleme
- le nombre de cluster peut etre fixe a l'avance par usr,
ou il faut le trouver aussi


## exemple

[](/classes/dcd/clustering.002.png)

- soit des objets definis par 2 attributs, coordonnees dans un plan
- on peut utiliser la distance euclidienne comme mesure de similarite
- relativement facile de separer les obj de l'exemple meme a l'oeil
- plus on a d'objets, plus c'est difficile,
notamment ici on a des jaunes "fringe"
dont la classification est moins sure
- l'enjeu est qu'est-ce qu'un bon clustering,
est-ce qu'on est sur d'avoir la bonne solution a chaque fois,
ou est-ce que c'est un probleme difficile et il faut utiliser des heuristiques


## inductive database vision

[](/classes/dcd/clustering.003.png)

- on peut voir le probleme comme pour la fouille de motifs
- on cherche les elements d'un langage tq verifie une contrainte dans les donnees
	{X ∈ P | Q(X,D)}
	avec P: langage (pattern space)
	Q: inductive query
- on peut se poser ici la question de ce que representent D, P et Q dans le clustering
- D: simplement un ensemble d'objets definis par des attributs
et munis d'une mesure de similarite
- les objets peuvent etre tout simples, ou plus complexes:
graphes, sequences, etc
- les attr peuvent etre de tout type et mixtes
- clairement, la mesure de similarite choisie depend des attr ± objets,
zB distances entre graphes

- P est l'ensemble de toutes les partitions possibles
- on suppose un nombre k de partitions defini par usr,
mais on peut avoir des cas ou il n'est pas fixe a l'avance,
mais varie entre 1 et le nombre d'objets
- on impose un certain nombre de regles:
	* les partitions sont des partitions d'un point de vue mathematique
	1) disjointes: pas d'intersection entre partitions
	2) chaque objet doit etre affecte a un cluster
	3) un cluster ne peut pas etre vide

- Q est une fonction a optimiser,
qu'on peut definir de plein de manieres differentes
- elle cherche a capturer a quel point
les objets a l'interieur d'un meme cluster sont similaires
et ceux a l'exterieur dissimilaires,
donc il faut capturer simultanement les deux

- plein de variantes existent
- on peut faire zB du clustering flou,
ou un objet peut appartenir a plusiers clusters
a partir d'une certaine valeur d'appartenance,
et donc l'intersection entre clusters n'est plus forcement vide
- donc en gros on veut retourner le meilleur clustering,
en fonction d'une fonction Q qui mesure la qualite du clustering


## algorithme naif

[](/classes/dcd/clustering.004.png)
