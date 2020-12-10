- entrepots de donnes, data graveyards
	* facons groupees de voir les donnes: sql group by
	* stockage sur une dimension particuliere: geographie, type d'acheteur, etc.
	* on definit carrement des langages OLAP
		* online analysis processing
		* traitement d'analyse de donnes
	* OLTP avant: transactions
	* pas encore du data mining
	* decouvertes de connaissance sur les bases pas faites pour ca a la base
- explicite: cahier des charges et stockage sous un certain modele
- a partir des annees 90: on va essayer d'aller plus loin
	* recherche de connaissances plus implicite
- cahier des nuggets(?): plein de ressources ml/data mining, offres d'emplois internationaux, maintenu par experts
- naissance d'une discipline, annees 90: DCD/data mining/fouille de donnes sur grande qte de donnes
- stats: distributions, stat descriptives, etc: modelisation des donnees
	* -> analyses, decisions, etc.
- et disciplines plus info: stockage des donnes: algo pour traiter de gros volumes de donnees
- ml: construction de classifiers, etc: remplacement des dernieres donnees
	* -> classification de nouveaux individus
	* -> sous groupes dans une population par rapport a des distances ou caracteristiques interessantes
	* supervisee: on connait les groupes a l'avance
- autres donnees que 3V:
	* donnees sales: incoherences, NA
	* etc: ⇒ autres sources de complexite
	* "big" etant juste une source de complexite parmis d'autres
- mapreduce: division du travail d'exploitation de donnees
- dcd: analyse de donnees dans un contexte big data
- visualisation de donnees, etc.
- connaissance implicite
	* web ca peut etre vu comme quoi?
		* pas un entropot de donnees
		* au pov structurel, c'est un immense graphe
		* ses noeuds sont un ensemble de documents
			* texte ou semi-structure (balise), mais balises souples
			* plus structure comme xml: balises souples mais syntaxe plus rigoureuse
			* medias
	* moteur de recherche -> sous ensemble du web: ensemble de documents du web
	* on n'est pas dans la recherche d'information, mais une selection d'infos parmis d'autres
		* on n'est donc pas dans l'explicite
	* ici on ne cherche pas des sous-ensembles, mais des comportements, modeles, classifiers
	* google search: utilise des tech de dcd/big data
		* chercheurs de google contributeurs en data mining
	* ce qu'il envoie n'est pas du dcd, mais sousjacent: data mining en cherchant des structures
		* recherche de modeles, structures dans les donnees, comportements etc
	* ⇒ se poser la question: qu'est-ce que sont les donnees, les connaissances, etc.
		* car peut-etre pas dans un probleme de fouille de donnees
	* ⇒ dm: on veut extraire des donnees implicites interessantes
- on definit ce qu'on essaie de rechercher:
	* zB classifier (sur donnees -> prediction sur nouvelles)
	* clustering, etc.
- espace de recherche: ensemble de toutes les possibilites en output
	* zB ensemble de tous les classifiers possibles
		* arbre de decision: exemple de classifier
			* si ca et ca et ca -> decision
	* ⇒ exponentiel
- bien distinguer espace de recherche et donnees
	* espace de recheche != les donnees
- espace de recherche a confronter aux donnees
	* ⇒ 2 problemes a se poser, 2 sources de complexite
	* lien entre les deux: mesures de qualite -> fonction de score
		* zB bon a 20%
- algo glouton: une technique sur des espace de recherche expo
	* parcours cet espace dans une direction sans jamais revenir en arriere
		* ne reviens jamais sur ses choix
		* score -> decide de la direction par la suite
	* ⇒ zB kmeans
		* ne reconsidere pas les points classes avant
		* ⇒ optima locaux
- ⇒ representation formelle des donnees (!= representation relationnelle == tuples)
- modelisation des 3 problemes ⇒ alors seulement on pose la question de resoudre le probleme
BI: business intelligence: entre info et economie

- exemple d'application: frequent itemsets mining (fr: analyse du panier des menageres)
	* donnees: il n'y a que ca, les articles des tickets de caisse
		* si l'ordre etait important, on aurait des "sequences d'achat"
		* pas ici
- zB: quels sont les articles achetes ensemble?
- ensemble d'articles possibles = {a,b,c,d,e}
- ensemble de tickets de caisse: ensemble d'articles
- ⇒ on veut la combinaison d'articles apparaissant ensemble
- ⇒ formalisation
	* faut specifier les donnees:
		* soit A un ensemble d'articles
		* soit t une transaction, cad un ensemble d'articles
			* t ⊆ A
			* ⊆: is a subset of
		* pas de notion d'ordre: ABCD ≡ CBAD
			* ≡: is identical to
		* on utilise ceci pour enumerer par ordre alphabetique pour nous simplifier la vie
		* soit d une base de donnees de transactions: ensembles de transactions
	* output: connaissances/motifs == ensembles d'articles
		* soit X un ensemble d'articles, X ⊆ A
			* !! on ne recherche pas des transactions! mais les transactions sont du meme type
			* motifs de sortie comparables avec les transactions, meme nature
			* t == donnees de depart
			* mettons qu'il existe X = {B}, X n'est pas une transaction
			* mais chaque transaction est forcement un motif
			* X est une possibilite que l'ensemble des t peut ne pas contenir
	* reste a definir le/les scores
		* soit sup une fonction de support qui va toujours de l'espace de recherche et lui associe 
			* espace de recherche != donnees
			* espace de recherche == tous les X possibles
			* soit P(A) |-> [0;1]
			* sup(X) |-> nombre de t ∈ d tq X ⊆ T / taille de d -> on ramene a un nombre entre 0 et 1
				* X |-> sup(X) = |{t ∈ d, X ⊆ t}| / |d|
		* on veut les groupes d'articles qui apparaissent dans au moins le tiers des transactions (zB)
			* ⇒ on definit juste comment voir les articles qui sont frequemment ensemble
- ⇒ on peut maintenant definir le probleme
	* on va prendre la formulation la plus simple, mais on peut faire des ameliorations
	* on va demander a l'expert un seuil: minsup
	* probleme d'enumeration (terme non anodin: on veut donner plein de resultats),
		* et non d'optimisation (exploration combinatoire et un seul resultat, l'optimum)
	* ⇒ modele:
	* soit d une bdd de transactions
	* soit minsup un seuil
	* on veut enumerer les ensembles X tels que sup(X) ≥ minsup
- sur l'exemple:
- pour BD, le nombre d'occurrences est de 4 parmis 9 ⇒ "support de BD est 4/9"
- support de ABE = 0
- approche naive:
	* foreach X ⊆ A		// ou X ⊆ P(A) -> complexite evidente
		* if sup(X) ≥ minsup
			* then ok
	* ⇒ un test par X
	* le cout du test n'est pas O(1),
	* mais juste en regardant le nombre de fois qu'on fait la boucle
	* ⇒ faut parcourir P(A)
	* |P(A)| = 2^|A|
		* est-ce qu'on a A? -> deux possibilites
		* etc
	* notation anglosaxone de |P(A)| ≡ 2^A
	* 5000 articles -> 2^5000 ~= 10^1500 tests
		* exponentiel ⇒ intractable
		* nombre d'atomes dans l'univers estime: 10^80
		* etoiles: 10^17
	* et a chaque fois il faut compter le score...
	* l'espace de recherche est toujours exponentiel, et donc on ne le parcours jamais
		* et on ne le represente pas (sur l'ordinateur)
		* a aucun moment il n'y a une representation physique de 2^A
	* ⇒ espace theorique a parcourir
	* faut donc remplacer foreach X par une autre facon, structuree et intelligente de parcourir P(A)
	* on veut une solution en temps lineaire (sortie rapide)
	* meme le polynomial dans du big data est enorme
	* on veut donc des comportements lineaires dans l'entree et dans la sortie
		* sinon ca passera pas a l'echelle
	* quand on fait de l'enumeration, on veut une complexite lineaire dans la sortie (en plus que l'entree)
	* enumerer tous les ensembles frequents: sortie exponentielle, il faut enumerer la sortie alors qu'elle est deja inexprimable
		* on se pose tj la question de la taille de la sortie en enumeration
		* on peut tout a fait obtenir des dizaines de milliers d'ensembles, etc
		* svt des sous problemes en fait, on raccorde la sortie a d'autres traitements
	* on veut que si la sortie est petite, de la trouver rapidement
- deja il faut parcourir P(A) et trouver des couples
	* les parties de P(A) qui interesse sont celles qui se retrouvent dans la sortie
	* plus vite on arrive a la sortie plus on est content
	* ⇒ on se pose le probleme de structuration
	* mise en forme sous un graphe
	* ⇒ on va ordonner, ordre naturel dans un ensemble == inclusion
	* inclusion de la fonction de score
- on va considerer P(A) comme etant (P(A), ⊆): ensemble ordonne
	* pas un ensemble total == ordre partiel
		* tout le monde n'est pas comparable dans un ordre partiel
	* on peut dire que AB ⊆ ABE, et AB ⊈ ACD, donc incomparables, on ne peut pas dire que l'un est plus petit que l'autre
	* ⇒ on peut soit dire qu'un est plus petit que l'autre, ou qu'ils sont incomparables
- ⇒ ensemble ordonne -> representation sous forme de graphe
	* relations est inclu dans etc -> ca devient un graphe
"treilli":
	* ABCDE 				// complementaire ∅(?), on prend tout
	* ABCD ABCE ABDE ACDE BCDE
	* ABC ABD ABE ACD ACE ADE BCD BCE BDE CDE
	* AB AC AD AE BC BD BE CD CE DE	// niveau 2: seules facons de choisir 2 (toujours le meme nombre car on choisit toujours les complementaires)
		* A B C D E
		* \   |   /
		*     ∅
	* (avec des traits d'inclusion de chaque element a tout de la couche inferieure)
	* combinaison == ensemble
	* arrangement != combinaison (permutation)
- ⇒ des qu'on prend 2 ensembles, ils ont une borne inf (intersection) et sup (union)
	* ⇒ "treilli"
	* niveau du dessus: complementaires -> meme nombre
	* a chaque fois on n'en met pas un
- largeur: combien on en a a chaque niveau
	* C_n^p:
		* n: nb total de niveaux
		* p: a quel niveau on se trouve
	* formule generale: n! / p!(n-p)!
	* si on fait la somme des C_n^p pour n de 0 a 5 -> 2^n
	* au milieu, ca va etre tres tres gros
- ⇒ c'est pour ca que c'est symetrique: si on remplace p par n-p, la formule de C_n^p donne le meme resultat (denominateur)
- ⇒ definition espace de recherche
- maintenant, comment parcourir cet espace de recherche? trouver une bonne propriete pour raccourcir? association avec le score?
- donner la liste de tous les ensembles frequents -> on veut un resultat exact (on veut etre sur)
	* donc heuristique pas bon, une solution approx n'est pas assez
- faut eviter au max les supports non-frequents
- on a deja vu des algos de parcours de graphes
- on pourrait essayer de couper le graphe a partir d'un certain noeud
- faut trouver une propriete reliant une fonction de score et l'output(?)
- rappel: fonction de score
	* support de x = nombre de transactions tq X inclu dans t divise par toute la base (support relatif)
	* relation d'inclusion
	* +1 sur son support faut etre dans une transaction
	* esp de recherche: tout P(A) muni de l'inclusion
	* qd on monte dans le graphe, on monte dans qui nous contient
	* qd on descend, on descend dans des noeuds que l'on contient
	* meme niveau: pas de relation d'inclusion
- sup(X) = |{t /x inclu t}| / |d|
- ⇒ propriete reliant l'inclusion et le support? c'est l'enjeu
- surtout on ne veut pas une heuristique (comme dans des techniques d'optimisation)
- et on veut eviter une approche naive exponentielle
- on a un minsup de specifie
- supposons qu'on a calcule le minsup pour un noeud
- que se passe-t-il pour ses voisins?
- quel est le comportement de la fonction de support quand on suit un lien d'inclusion?
- on veut trouver des proprietes d'ordre, de monotonie: croissance, decroissance
	* ⇒ on regarde s'il n'y a pas un sens de variation
- support de AB toujours inferieur au support de A ou B
	* si on a AB, on a A et B, mais la reciproque n'est pas vraie
- donc si on remonte, le support ne peut que decroitre
- pas une monotonie stricte malheureusement, on peut avoir un support equivalent
	* zB partout il y a A, il y a B -> B et AB ont le meme support
- si on disqualifie un noeud, on disqualifie tout les noeuds lies en haut
- c'est ce qu'on recherche: un moyen de ne pas parcourir tout le graphe
- ⇒ propriete, la seule qu'on va utiliser dans cette partie
	* -> pour problemes ou la fonction de score et l'esp de recherche sont lies par une propriete de monotonie
- sup P(A) |-> [0,1]
	* application qui va d'un espace ordonne a un espace ordonne
	* ⇒ monotonie, notions de terminale sur ℝ
	* si x' > x, f(x') > f(x)
	* si x inclu dans y, 2 elements ordonnes au depart		// on est dans P(A)
		* ⇒ on leur applique la fonction (des deux cotes)
		* sup(x) ≥ sup(y)						// on est dans ℝ
		* ⇒ monotonie decroissante
	* ⇒ seule propriete utilisee par l'algo utilise
- on veut des proprietes qui donnent un sens de recherche et un moyen de reduire le probleme
- soit x inclu dans y
- ⇒ y frequent implique x frequent
- contraposee de cette implication:
	* implication inverse ≠ implication contraposee
	* si A implique B, si on n'a pas B, on n'a pas A
- ⇒ x non frequent implique y non frequent
- on peut ainsi en deduire des strategies directes
- on va pas utiliser la propriete y frequent ⇒ x frequent dans notre algo
	* on va parcourir par le bas
	* c'est un choix justifie
	* on pourrait avoir des variantes ou occasions ou on devra partir par le haut
	* dans ce contexte et pour la question posee, on doit partir par le bas
	* on veut TOUS les frequents
2e image ⇒ solution du probleme: on sait qui est frequent et qui ne l'est pas
- on voit la monotonie apparaitre
- ensemble frequent vert en haut -> sous-elements frequents, propages par le bas
- non frequents propages depuis le haut
- ensembles frequents majoritairement
5000 articles -> treilli a 5000 niveau, et au milieu, plusieurs millions d'elements
	* est-ce que des gens acheteraient des millions d'articles?
	* est-ce que ces millions d'articles sont frequents?
	* vraisemblablement (on espere), les elements frequents sont plutot en bas du treilli
	* logiquement dans le contexte defini, on ne va pas avoir des caddy avec des milliers d'articles
	* les elements freq seront en bas, d'ou l'utilite de partir du bas
	* les frequences sont de toute facon dans la sortie et il faut les calculer et les ecrire dans un fichier
		* ce n'est pas eux qu'on veut eviter
		* donc si on part par le haut, on va en eviter
		* les non-frequents sont evites ainsi et on ne passe pas du temps de calcul dessus
		* on veut donner tous les ensembles frequents -> faut de toute facon les parcourir
- ecrivons l'algo de facon assez generique:
	* recherche par parcours par niveau
	* utilise dans d'autres contextes
	* entree: minsup, d
		* dans d on a A, A fait partie des donnees
	* on commence a preparer le niveau 1 qu'on doit de toute facon faire en integrale
		* niveau 0 -> aucun interet
		* on a besoin de resultats pour pouvoir exclure des noeuds
	* C1 -> candidats de niveau 1: on va tester tous les singletons
		* C1 <- {{a} / a appartient a A}
			* ensemble des singletons appartenant a A
	* on va faire une boucle qui monte par niveau
	* arret quand il n'y a plus de candidats
	* variable de boucle: i = 0
	* on va utiliser une fonction eval qui donne en sortie tous les frequents de taille i
		* prend chaque candidat et evalue son support
		* et prend le minsup
	* puis il nous faut des candidats pour l'iteration suivante

- i = 1	// niveau 1
	* C1 <- {{a} / a appartient a A}
- while Ci ≠ ⊘
	* Fi <- Eval(Ci, d, minsup)
	* Ci+1 <- GenCand(Fi)
	* i <- i + 1
- end while
- return ∪ Fi // union des Fi

- deux sources de complexite:
	* GenCand: gere l'espace de recherche, l'exponentielle
		* c'est ici qu'on coupe des parties de l'espace
		* a partir des frequents detectes, qui explore-t-on ou pas?
		* implementation depend de la structure de l'espace de recherche
			* ensemble de toutes les sequences possibles
			* ou tous les graphes possibles
			* etc
		* fonction qu'il faudra refaire en fonction de l'espace de recherche
		* a partir du moment ou on a un espace avec une monotonie,
			* on pourra faire ce genre de chose
	* Eval: acces aux donnes, svt de grande taille
		* depend du support des donnees: bdd, etc.
		* tres important de compter correctement
	* les diverses contributions se font a un des deux niveaux,
		* ou alors on essaie de reduire le probleme
- on ne va pas voir l'algorithmique des structures de donnees en detail
	* on pourrait avoir a implementer ce genre de chose
	* selon si on arrive a obtenir la bonne sortie, le temps d'execution sera tres different
	* structures de donnees, gestion de la memoire, facon d'implementer des sets, de les parcourir, etc
	* beaucoup de problemes a gerer

- prenons l'exemple et essayons de le derouler
- premiere etape: on genere C1: les singletons
	* A B C D E
- puis on va tester les ensembles
	* on n'a pas le choix, on rentre dans la boucle et on calcule le support de chacun
- selon les donnees: supports
- on va pas diviser par 9 le support a chaque fois, ca change rien,
	* au tableau on va travailler avec les valeurs absolues et non relatives
- dans la premiere boucle, tout le monde est frequent
GenCand fait les candidats de la deuxieme iteration
	* GenCand calcule C2 a partir de F1
	* ⇒ tous les sous-ensembles, car le niveau 1 frequents
- pour faire un candidat de niveau 3,
	* tous les sous ensembles du niveau 2 doivent etre presents
	* autant de possibilites que d'elements
- technique pour les generes utilise l'ordre alphabetique
	* s'ils sont tous la, on a toujours 2 qui ne different que par leur dernier element
	* vrai a tous les niveaux
	* zB ABC a 1 en enlevant C, 1 en enlevant B, 1 un enlevant A: AB, AC, BC
	* difference de 2 elements -> pas inclu, faut differer d'un seul element
	* AB et AC ne different que par le dernier element -> ABC, ABD possibles
	* puis avec AD, on peut faire ACD et il n'y a plus rien qui commence avec A
	* on a trouve les deux elements qui commencent pareil
	* on doit verifier la presence des ensembles generes
	* prenons ADE: il n'est pas candidat car il a des sous-ensembles non frequents
		* on n'a pas passe de temps de calcul dessus
		* on est passe directement sur les candidats
		* donc certains on n'en parle meme pas
		* important car ca nous evite de passer du temps dessus
	* ABC se combine avec qui? ABD -> ABCD
	* on verifie: ABC, BCD sont frequents -> ABCD est bien un candidat
	* ACD personne, BCD personne, CDE personne
	* on ne peut pas monter plus haut que C4 -> algo arrete a C5
- C1	sup	F1	C2	sup	F2	C3	sup	F3	C4	sup	C5
- A	6	A	AB	3	AB	ABC	3	ABC	ABCD	3	⊘
- B	4	B	AC	6	AC	ABD	3	ABD
- C	8	C	AD	6	AD	ACD	6	ACD
- D	8	D	AE	2	BC	BCD	4	BCD
- E	5	E	BC	4	BD	CDE	4	CDE
			* BD	4	CD
			* BE	1	CE
			* CD	8	DE
			* CE	3
			* DE	4
- ensemble des frequences: F = {A,B,C,...AB,BC,...,ABCD}	// tous ceux qui sont dans les frequents
	* == sortie de l'algorithme: tous les frequents
	* correspond a la partie verte du treilli avec la solution
- pour minsup, on fait toujours ≥
- ⇒ faut savoir faire ca pour l'examen: base de transactions et derouler le truc
	* on n'aura pas le graphe, on ne va pas dessiner tout le graphe
	* justement, l'algo ne definit/genere pas le graphe, pas possible
	* structurer comme ci-dessus: Ci_sup	Fi	Ci+1_sup	etc
	* (sup en subscript du Ci)
- nouvel exercice
- ACD
- BCE
- ABCE
- BE
- ABCE
- BCE
- minsup = 2

- A,3	A	AB,2	AB	ABC,2	ABC	ABCE,2	ABCE
- B,5	B	AC,3	AC	ABE,2	ABE
- C,5	C	AE,2	AE	ACE,2	ACE
- D,1	E	BC,4	BC	BCE,4	BCE
- E,5		BE,5	BE	
	- CE,4	CE
- S = {A,B,C,E,AB,AC,AE,BC,BE,CE,ABC,ABE,ACE,BCE,ABCE}

- C1 -> prefixe commun = ⊘, donc tout le monde peut combiner avec tout le monde
	* AB + AC
	* AB + AE
	* AC + AE
	* AE -> ⊘
	* BC + BE
	* BE -> ⊘
	* CE -> ⊘
	* ABC + ABE
	* ABE -> ⊘
	* ACE -> ⊘
	* BCE -> ⊘
	* ABCE -> on ne peu pas faire plus -> arret de l'algo
- on ne peut pas monter jusqu'en haut a part si on a tout parcouru en bas
	* == tout est frequent
	* l'algo ne s'arretera jamais
	* la complexite est par rappor a la sortie
	* minsup trop bas -> frequent tres haut ⇒ on n'y arrivera pas (dans cette forme du probleme)
	* on ne peut pas enumerer tous les frequents si on a genre un frequent a 100
		* il faudrait enumerer tout ce qui est aux niveaux en dessous -> 2^100
	* qd grands frequents toujours interessant, mais cette methode la n'est pas pratique
- limitation
	* s'il existe un tres grand frequent, on n'y arrivera pas, algo en echec
	* monter jusqu'a lui, on ne pourra parcourir ces sous-ensemble parce qu'il y en a 2^n
	* de toute facon tout autre algo sera en echec car si on veut tous les sous-ensembles,
		* la sortie sera inexprimable
	* ⇒ c'est pas l'algo le probleme, la question elle-meme est en echec
	* qq soit l'algorithme qu'on met, on ne pourra pas exprimer la sortie
	* autre limitation: minsup
		* introduire un minsup est une limitation, ca pose probleme
		* car on est face aux donnees, et il faut decider ce qu'on met en entree
		* depend beaucoup des donnees, et c'est du tatonnement
		* ca marche pas, l'algo ne se termine pas -> faut mettre un minsup plus grand
		* jusqu'a avoir une sortie bonne
		* pas satisfaisant pour un expert
		* on doit arreter en force un algo qui tourne en boucle
- on aura des variantes du probleme qui conduisent a des strategies a poser des questions plus adaptees
- on peut fixer la taille de la sortie: les n les plus frequents
	* pas de minsup a fixer, plus aise pour l'expert
	* algos un peu differents, mais meme espace de recherche
	* ou algos identiques mais utilises differemment
	* -> "top k": les k ensembles les meilleurs, les plus frequents
	* -> on demande des representations des frequents: "representations condensees"
		* formellement, on "resume" les frequents
	* voire une sous-partie des frequents
		* zB les plus grands en taille
			* ⇒ autres strategies
			* si vraiment tres grand, on pourra commencer par le haut
			* ou strategies en profondeur plutot qu'en largeur
				* on attaque un chemin et on avance tant qu'on est frequent
				* depth-first search vs breadth-first search
				* (algos de parcours de graphes)
	* on peut faire des approches hybrides par le haut et par le bas
	* ou en faisant des sauts dans l'espace de recherche, pour tenter
	* tas d'algo existants guides par ce que l'on recherche
- monotonie(?) -> 2 sous-ensembles interessants
	* dernier slide (colore) -> elements en gras et soulignes
	* on va parler de frontieres
	* constat: des qu'on a un grand frequent, tous les sous-ensembles sont frequent
	* donc il suffit de le donner a l'expert
	* donc on peut les resumer par les frequents les plus grands
	* S = ensemble des frequents (les elements en vert)
		* on essaie de peut-etre resumer par ses elements maximaux
		* on definit une "frontiere positive" dans l'espace,
		* en dessus de laquelle c'est vrai et en dessous c'est faux
		* Bd pour border
		* mesure monotone decroissante
		* frequents bordure positive des grands frequents
		* des que tous ses surensembles ne sont pas frequents
		* des qu'on passe au dessus de lui, par quelque soit le chemin,
		* alors on parle de frontiere des frequents
		* Bd+ (F) = {X appartenant a F / pour tout X' contenant X, alors X' appartient a Fbarre (non-freq)}
			* = {max par inclusion des F}
		* notation Bd+(F) = {X ∈ F / ∀X' ⊇ X, X' ∈ Fbar} = max_⊆(F)
	* ex1: Bd+ = {ABCD, CDE}
		* tout element vert est un sous-ensemble d'un de ces deux ensembles
		* tous sont couverts par un de ces ensembles
		* a l'inverse, eux ne sont pas couverts par des ensembles plus grands
			* plus grand au sens de l'inclusion
		* cette frontiere est interessante
		* ens inclu dans 1 des 2 ⇒ frequent
	* = representation/resume exact de S
		* grace au fait que le score est decroissant
	* si on change la question a trouver la bordure positive des frequents
		* ⇒ la meilleure technique n'est pas l'algo qu'on a vu
		* ni de partir par le haut
		* ∃ autres techniques plus complexes
			* pour simplifier, techniques qui font des sauts dans l'espace de recherche
			* sauts a temps lineaire
	* pourquoi est-ce qu'on ne ferait pas toujours ca?
		* qd meme une perte non-negligeable
		* sortie avant pouvait donner tous les frequents + leur support
		* information importante pour l'expert ou pour les problemes pour lesquel ce probleme est un sous-probleme
	* represente tous les frequents, mais pas leurs supports
		* donc il y a une perte
	* ∃ des representations condensees sans perte, mais on ne va pas les detailler
		* il y a des algo dedies
		* notamment "ensembles fermes", "ensembles libres"
		* par contre, on aura plus que les bordures
		* ensembles plus riches -> on trouve le support sans acces aux donnees, etc
	* on ne peut pas faire plus petit que la bordure
- autre ensemble, mais dual a celui-ci, lui mais pris dans l'autre sens
	* = frontiere negative Bd-
	* on peut regarder la meme chose par la vision verte ou la vision rouge
	* espaces clos par le haut ou par le bas (car monotone), ici bas
	* on resume donc les rouges
	* Bd- = {AE, BE} = les plus petits
	* si on nous donnent tous les rouges,
		* on peut retrouver tous les verts
		* == ceux qui ne contiennent pas les non frequents
		* tous ceux qui ne contiennent pas AE et BE
	* parfois plus petite, parfois pas par rapport a Bd+
	* pas de regle la-dessus
	* Bd-(F) = {X ∈ Fbar / ∀X' ⊆ X, X' ∈ F} = min_⊆(Fbar)
		* tous leurs sous-ensembles sont frequents
		* (Bd+: derniers verts, Bd-: premiers faux)
		* = minimaux par inclusion de Fbar
- on a des techniques algorithmiques pour calculer Bd+ par Bd-
	* mais algos non-polynomiaux
- l'algo qu'on a vu s'appelle "A priori", ~1994
	* le plus connu des algorithmes par niveau
	* dans le domaine d'extraction de connaissances
	* c'est leur prototype
	* un des plus efficaces qd bien implemente
A priori fournit F mais a quel prix? qu'a-t-il parcouru?
- quels sont tous les elements pour lesquels on a du calculer leur support
	* tous les verts, forcement, ils ont tous ete candidats
	* jusqu'au premier rouge
	* ⇒ cout == parcours de tout F + Bd-(F)
		* on peut dire qu'il donne Bd+ du coup aussi,
		* mais apres du post-processing
	* Bd-: tous ceux qu'on a teste qui sont faux
	* tous les autres negatifs n'ont jamais ete candidats
	* Bd-(F) est le prix a payer,
	* il faut parcourir toute la bordure pour discriminer la partie fausse
	* par ailleurs il peut facilement faire partie de la sortie
		* meme algo avec tres peu de modifications
	* -> autre probleme: on ne recherche plus les elements frequents,
		* mais les comportements rares
		* comportements rares sont couverts par Bd-
		* meme algo! donne tous les positifs, mais a parcouru tout Bd- aussi

- on va introduire une connaissance un peu plus riche que les ensembles frequents
	* probleme tres lie car meme contexte
	* ⇒ extraction des regles d'associations
		* plus connu que les frequents
		* cote outil, grand public
			* on va nous parler de regles d'associations
- nouveau probleme: faut d'abord definir le contexte, ce qu'on cherche
- le contexte des donnees est exactement le meme
	* tickets de caisse, meme contexte, on n'enrichit pas les donnees
	* mais on enrichit les connaissances, choses un peu plus interessantes
- c'est marrant de savoir quels articles sont achetes ensemble
	* mais limite niveau decisionnel
- plutot, est-ce qu'on pourrait deduire des articles a partir d'autres articles
- connaissances locales toujours, on ne cherche pas un classifier etc
- on cherche des comportements locaux
	* gens qui achetent a,b,c achetent aussi b,d,e?
	* qui ont telles proprietes ont aussi telles autres proprietes?
- on predit a partir de certaines proprietes d'autres proprietes
	* ⇒ on va definir des regles
- ⇒ on cherche des regles entre ensembles d'articles
	* zB amazon: proposition d'autres articles associes suite a un achat
	* ou site web: deviner des raccourcis, ou mettre des pubs au bon endroit
	* ou associations entre expressions de genes
	* ou entre diagnostics de patients etc
- les donnees on les a deja dans la formalisation
	* donnees formelles: bdd de transactions: BdT, multiensemble de transactions, chaque ensemble etant une transaction d'articles
	* mais faut changer les connaissances, on ne cherche plus des ensembles d'articles
	* connaissances: expressions (regles) du type x ⇒ y, avec x et y ⊆ A
		* "expression": point de vue purement syntaxique
	* on peut demander que X ∩ Y = ⊘ pour eviter des problemes
- on va faire deux fonctions de score, des sortes de filtres des donnees
	* on va garder ce qui est un peu frequent ⇒ premiere fonction == support
		* si on a deux transaction associees une fois -> difficile d'en deduire une regle quelconque
	* sup(X|->Y) = sup(XY)
		* nombre de transactions qui contiennent l'antecedent et la consequence
		* sup(AB|->C) = sup(ABC)
	* mais si on s'arrete la, sup(X|->Y) = sup(Y|->X)
	* il nous faut une autre fonction plus fine qui evalue les regles
		* une multitude de fonctions existent,
		* on va prendre la classique
		* d'autres vont affiner en fonction de ce qu'on recherche, par exemple des correlations
			* corr sont differentes: event entre evenements aleatoires
			* zB tout le monde achete du pain
				* les gens qui achetent aussi une cafetiere -> association car le pain sera la systematiquement
				* mais pas de correlation
		* celle de base == "confiance"
	* conf(X|->Y) = proportion de gens qui effectivement achetent Y parmis ceux qui ont achetes X
		* = sup(XY) / sup(X)
		* compris dans [0,1]
	* monotonie decroissante -> sup(XY) ≤ sup(X)
	* oriente, contrairement a sup()
	* ca regle pas le probleme de la correlation, c'est juste une association
		* pain implique cafetiere? ⇒ bcp de pain, peu de cafetiere ⇒ confiance faible
		* cafetiere implique pain? ⇒ confiance de 1, certain
- enonce du probleme:
	* soit d une bdd de transaction,
	* soit 2 seuils minsup ET minconf,
	* donner toutes les transactions qui passent
	* 2 barres a passer, et le passage d'une n'implique pas le passage de l'autre
	* deux regles complementaires, mais independantes
		* (cf enregistrement..)
		* si on ne tient pas compte du minconf, c'est comme si on mettait un minconf tres petit -> multitude de regles
		* on peut avoir un tres grand support mais une confiance tres basse,
		* car sup(X) encore tres vaste par rapport a sup(XY) meme si XY arrive souvent
			* on peut avoir XY qui arrive souvent comme avec le pain,
			* mais X tout seul qui arrive presque tout le temps
			* ⇒ tres petite confiance, tres grand support
- ⇒ autre cadre, y compris dans les problemes dans les cours suivants
	* autres problemes de fouille
	* si on ne les donne pas precisement, faut toujours a reflechir quels sont les donnees, les sorties, les scores, etc
	* la plupart des algos font d'abord le support, puis la confiance
	* tous les X possibles sont un treilli, tous les Y possibles aussi
		* ⇒ encore plus gros que le treilli des parties, toujours exponentiel
		* mais bon depend de comment on pose le probleme
- resolution:
	* 1. on recherche F
		* l'algo qu'on a vu est un sous-probleme de celui-ci
		* les regles frequentes sont deja dans les frequents
		* si ABC est frequent, dans toutes les regles A->B, AB->C etc sont frequentes
	* 2. parmis les frequents x ∈ F, on extrait les regles d'associations RA
- la partie lourde algorithmique est toujours la 1
	* c'est toujours l'algorithme a priori
- en general les outils commerciaux ne proposent pas de trouver les frequents, c'est surtout un sous-probleme
- en realite l'algo ne donne pas les frequents, mais les associations
- mais le probleme le plus difficile est toujours 1 -> plus de recherche dans le milieu scientifique
- mais RA plus interessantes -> connu grand publique
- il faut les frequents et la frequence: il faut connaitre tous les supports
	* donc juste la Bd+ n'est pas suffisante
- on est dans le cas ou le sous-probleme interessant est de donner les frequents avec leurs supports
- les fonctions de score peuvent varier selon ce que veulent les experts
