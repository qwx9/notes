# preference-based pattern mining

- Preference_Based_PM_eBISS.pdf
- first slides focus on historical context,
needs, applications, definition of data mining
- could be useful to reread it prior to exam


## historical context

- until 1600s: empirical science
- 1600-1950s: new theoretical component to each discipline
	* theoretical models motivating experiments and generalizing knowledge
- 1950-1990s: new computational component
	* simulations grown out of inability to find
	closed form solutions for complex mathematical models
- 1990-now: data science era
	* flood of data from new instruments and simulations
	* ability to economically store PB's of data
	* universal accessibility via internet
	* information management, acquisition, organization, query, visualization
	* "the fourth paradigm", data-intensive scientific discovery
- evolution of database systems
	* dbms → rdbms
	→ advanced rdbms (oo, extended r, deductive etc)
	→ application-oriented dbms (spatial, engineering etc)
	→ data mining, warehousing, media/web databases
	→ stream data management/mining, web, nosql, newsql, etc


## data mining

- "we are drowning in data, but starving for knowledge", john naisbitt
	* explosive growth of data
	* automation, availability
	* from business, science, social media, etc
- knowledge discovery from data (kdd)
	* extraction of interesting patterns or knowledge from huge data
	* interesting = non-trivial, implicit, previously unknown, potentially useful
	* aka knowledge extraction
	* aka data/pattern analysis
	* aka data archeology
	* aka data dredging
	* aka information harvesting
	* aka business intelligence
- watch out: data mining?
	* simple search, query processing
	* (deductive) expert systems
- data mining is the core of kdd: search for knowledge in data
- two methods: descriptive vs predictive data mining
	* predict, estimate future events: learn a model
- tons of uses
	* = prediction, classification
	* outlier detection
	* and other functions
- many challenges
	* mining methodology
	* scaling
		. as we've seen,
		usually the major obstacle is not the size of the data
		(linear complexity)
		. but the size of the search space
		(exponential complexity)
	* etc


## difference between data mining and machine learning

- none, really
	* buzzwords, use depending on trends
	* one difference: predictive (global) modeling vs. exploratory analysis
- predictive modeling
	* construct the most accurate model based on data
	* ultimate purpose: automate a process
- ecd
	* decouverte de nouvelles connaissances
	dans des domaines dans lesquels les donnees ont ete recuperees
	* pour booster les connaissances humaines
	* objectif d'apprentissage humain, pas de prediction
	* la, on veut des choses interpretables
	≠ choses tres precises cote ml
- certains modeles sont facilement interpretables ET predictifs
	* zB regression lineaire

[lm](preference.based.pattern.mining.001.png)

	* rien qu'avec pente de regression ⇒ bcp d'information
	* mais sur cet exemple, la regression fit tres peu aux donnees
	* modeles qui fit bcp plus: mixture gaussienne
		decrit bien les donnees mais bcp moins interpretables
		la pour connaitre le processus, il faudrait connaitre tous les coefs
		ici ≥ 300
	* on a toujours ce compromis

avec les motifs frequents, on peut deja trouver des choses interpretables
	* dans des langages extremement simples,
	on fait des conjonctions d'items
	* on peut faire des langages bcp plus sophistiques
	* conjonctions de conditions sur attr numeriques
	* ⇒ zB identification de polygones paralleles aux axes

[](/classes/dcd/preference.based.pattern.mining.002.png)

	* ici on peut facilement identifier
	avec une condition
	que la valeur theorique doit etre < 0.2
	* on peut deja trouver des choses interessantes
	* zB sur cet exemple globalement la valeur moyenne sur y = 0.43
	* avec une condition, on peut identifier un ensemble d'objets
	(sous-groupe)
	qui ont a peu pres tous la meme valeur sur y = 0.05
	* on peut faire des conditions plus sophistiquees
	* zB variable > 0.8 + condition sur forme des points (ronds)
	* on trouve un sousgroupe de points
	qui ont une valeur constante sur y autour de 0.75
	* on peut le faire avec des fouilles de motifs
	* car le luxe qu'il y a est qu'on peut dire
	que ca c'est interessant
	et le reste on sait pas
	* pouvoir ne pas expliquer tous les points des donnees est un luxe
	* car dans des modeles predictifs,
	on va essayer de construire un modele sur les donnees pour bien predire

[](/classes/dcd/preference.based.pattern.mining.003.png)

	* a gauche: arbre de decision optimal
	* a la racine: toutes les donnees
	* a chaque branche: conjonction de conditions
	qui permet d'arriver sur une feuille
	* ou on a les objets d'une classe tres majoritaire,
	voire quasi-pure
	* etant un objet qui arrive,
	on pourra le classer
	* zB malade/sain, ou attaque tcp/ip ou pas
	* construction d'un arbre sur l'ensemble des donnees d'apprentissage
	pour ensuite etre utilise en pratique
	* a chaque fois on optimise une fonction cout
	* zB minimisation de l'erreur de classification
	* l'arbre de decision est oblige des choix heuristiques
	* car l'ens de tous les arbres possible est enorme
	* zB ici on selectionne quand on construit notre arbre
	l'attr et condt dessus qui separe le mieux les donnees
	* de facon iterative
	* jusqu'a obtenir des feuilles quasi-pures
	* ici on a l'arbre optimal pour ce jeu de donnees
	* probleme: precision de 80%
	* avec des approches motifs/sousgroupes,
	on peut identifier des sousgroupes
	avec des condt tres simples
	qui ont une accuracy locale de 100%
	donc purs
	* car on selectionne les sousgroupes interessants
	* et on ne cherche pas a expliquer le reste
	* donc on n'essaie pas d'optimiser une fonction sur toutes les donnees
	⇒ permet pas mal de degres de liberte
	et de trouver des choses relativement interessantes dans les donnees

avec les motifs/sous-groupes, on peut aller plus loin
	* on peut trouver des choses dites exceptionnelles dans les donnees
	* on utilise des modeles permettant de modeliser les donnees dans leur ensemble
	* et ce meme modele est utilise pour voir ce qui se passe localement dans les donnees
	* zB lm pour representer les donnees

[](/classes/dcd/preference.based.pattern.mining.004.png)

	⇒ correlation positive entre y2 et y1
	* ce qui est globalement verifie par l'ensemble des donnees
	* on peut maintenant identifier des sousgroupes
	ou si on construit un modele que sur ces objets la,
	qqch de totalement diff: anti-correlation
	⇒ modeles "exceptionnels"
	⇒ on verra ca en TP et dans d'autres cours

tout ce qui est decrit en dcd
peut etre decrit en termes de bdd inductives
	* extremement simple
	* etant donne un langage, une bdd, des contraintes
	* on calcule l'ensemble des elements d'un langage
	qui verifient les contraintes dans les donnees

[](/classes/dcd/preference.based.pattern.mining.005.png)

	* la notion de langage est ce qui nous permet
	de representer les connaissances en gros
	* pour l'instant les langages qu'on a vu c'est les itemset
	= conjonctions d'items
	* les donnees ne sont utilisees finalement
	que pour verifier la qualite de l'itemset/motif dans les donnees
	* quand on fait des requetes
	en fait c'est des parcours de la bdd
	* la on fait un parcours d'un espace de recherche
	* pour les elements juges pertinents,
	on verifie la qualite dans les donnees
	* bcp plus complique car espaces de recherche souvent exponentiels
	* si on resout certains problemes avec une approche bdd,
	il faudrait un nombre exponentiel de requetes pour le resoudre
	* representation sous forme de bdd inductive:
	tres simple et on a tout dedans
	* on peut considerer des langages plus complexes:
		. travailler sur des notions de sequences d'itemsets:
		suivi-de, en-meme-temps
		= plusieurs relations temporelles au sein de la seq
		. on peut travailler sur des langages de graphes,
		de sequences de graphes
		(= graphes dynamiques)
		. etc
	* les contraintes sont utilisees pour representer
	les contraintes de l'utilisateur sur les donnees
	* une contrainte qu'on a vu est la notion de frequence
	* mais on peut en avoir d'autres
	* si on travaille sur des graphes,
	on peut etre interesse par des graphes denses
	* dans des seq,
	on peut s'interesser a des choses qui se passent de facon periodique
	= contrainte periodique
	* ou sequences qui verifient une regexp
	⇒ plein de contraintes possibles
	* enjeu = exploiter au mieux ces contraintes dans les algo
	* le pire qui peut etre fait: approche naive
	* enumeration de toutes les solutions possibles
	et on retient les solutions pertinentes en post-ttt
	⇒ non-viable car on doit parcourir tous les el dans l'espace de recherche
	qui est exponentiel
	* l'idee est donc d'identifier le plus rapidement
	les elements de l'espace de recherche un element non-pertinent
	et pouvoir l'elaguer
	* tout l'enjeu est de pouvoir sortir des proprietes des contraintes
	pour les exploiter dans la recherche

## detour historique: constraint-based pattern mining: assez recent

- algo a priori: 1993
- alors que bdd relationnelles: annees 60-70
- au debut course a la milliseconde pres
pour avoir des algos les plus rapides que possible
pour les frequents
- on est content,
mais on est alles droit dans un mur
car les frequents ne sont pas une panacee
- car la sortie peut etre plus importante de l'entree
- car on est tributaire du seuil choisi
et on peut donc sortir un nombre exponentiel de solutions
- imaginons qu'un motif est une hypothese a tester
e.g. connaissance potentielle
qu'on pourrait tester sur paillasse
⇒ temps, argent
- nombre exponentiel de tests => impensable
- fin '90: on s'est rendu compte de ca
- il faut avoir un nombre reduit de sorties
et pas faire que les frequents
et utiliser d'autres contraintes
- ere de la fouille de donnees sous contraintes
- hypothese forte faite ici:
l'utilisateur est capable d'exprimer ses interets
sous forme de contraintes
- notamment avec des contraintes sur des seuils
- zB quand on parle de frequents, on parle de seuil
- et fixer un seuil est tres complique
- seuil trop gros: peu de sorties
mais on a que des choses qu'on connait deja
= tautologies
- seuil trop bas: quasiment tous sont frequents
= inexploitable
- tres complique de fixer des seuils
et de representer la situation sous forme de contraintes
- shift: plutot que de voir le probleme
comme un probleme de satisfaction de contraintes,
on peut le voir comme un probleme d'optimisation
- recherche directe d'ensembles de motifs qui optimisent quelque chose,
zB notion de couverture
ou top-k par rapport a une mesure
- plutot que de representer sous forme de contraintes,
representation sous forme de preference
- plus la valeur sur cette mesure est importante,
plus ca m'interesse, etc
- si plusieurs preferences en meme temps,
on peut etre amene a chercher un prompt par etau(?)
- on cherche des motifs qui optimisent des preferences de l'utilisateur
- cool car emettre des pref est plus facile que des contraintes
- mais pas toujours trivial
- derniers travaux: on apprend les preferences de l'utilisateur
en meme temps qu'on fait de l'exploration
- approches interactives
- on propose des motifs a l'utilisateur
qui emet du feedback dessus
a partir duquel on apprend un modele de preference
et on l'integre pour trouver des motifs plus pertinents
- necessite d'arriver a bien capturer les preferences de l'utilisateur
⇒ machine learning etc
- mais aussi des boucles d'interactions avec l'utilisateur
- donc on ne faut pas attendre longtemps quand on lance l'algo,
mais des resultats instantanes
- chercher un motif frequent = np-complet
- recherche de l'ensemble complet de motifs frequents,
meme avec des heuristiques
→ difficile d'avoir des garanties en temps
pour avoir la solution
- donc on doit encore changer
⇒ problematiques d'echantillonnages des solutions
e.g. echantillonnage direct de l'espace de recherche
- bcp plus simple d'echantillonner que de chercher l'ens des solutions
rien qu'en termes de codage
- si on veut faire des algos corrects et complets,
faut les optimiser a mort
(structures de donnees etc)
- alors que le sampling c'est parfois 7-8 lignes de code et c'est bon


## recherche de motifs avec les frequents

- on passe les diapos ici, on l'a deja vu
- l'algo a priori est alle elaguer l'espace de recherche
en s'appuyant sur la propriete d'antimonotonie du support (freq)
- si un motif est freq, tous ses sousmotifs sont freq
- si un itemset est non-freq, tous ses superensembles sont non-freq
et on peut les elaguer
- vrai pour les itemsets mais aussi pour tous les langages (seq etc)
- on peut se ramener a structurer l'espace de recherche sous forme de treilli
dans lequel on a des el particuliers
- ensemble vide: le plus general, verifie par tous les elements de la bdd
- en bas: itemset le plus grand qu'on peut esperer avoir
= alphabet, ensemble des items
- on peut l'explorer de plein de facons
en largeur: en partant de ∅, puis niveau 1, puis niveau 2 etc
⇒ algo a priori, breadth-first (en largeur d'abord)
- recherche des items freq,
qu'on combine entre eux pour generer
les candidats de taille 2 pott freq, etc
- on peut aussi explorer en profondeur d'abord
	* on prend item1, s'il n'est pas bon, on passe (backtrack)
	* sinon on continue vers le bas
	* svt une branche tres profonde,
	et la derniere de profondeur = 1
⇒ pour des explorations correctes et completes
- on a d'autres heuristiques ou on perd la completude
- necessaire pfs pour rendre faisables les extractions
- beam search: recherche en faisceaux
	* taille de beam, zB k=2
	* parcours en largeur mais avec une taille de faisceau
	* niveau 1: on garde que les k meilleurs
	* et exploration a partir de ces deux la uniquement
	* et de meme pour les niveaux superieurs
	⇒ carottage de l'espace de recherche
- on peut aussi faire du sampling
	* on ne part pas necessairement de ∅
	* mais identification directe de zones dans l'espace de recherche
	* plein de formes de sampling
		. mcmc
		. sampling direct
		. mcts, recherches arborescentes de monte carlo
	* on a des espaces de recherche enormes
	et faut arriver a resoudre un compromis exploitation/exploration
	* on selectionne soit une branche qu'on va explorer car prometteuse
	soit on explore une nouvelle branche
	⇒ en general utilise pour les IA de jeux
		. google go: deep learning possible grace a mcts
		. world of warcraft etc
- un truc interessant:
qd on fait le parcours en prof,
on voit que la 1ere branche est extremmt profonde
et la derniere bcp moins
	* on a un espace de recherche qui ressemble a un triangle droit
	pointant vers le bas: |/
- on a des heuristiques d'enumeration
qui essaient d'equilibrer mieux ca
- fail first principle
	* on essaie d'exploiter la prop d'anti-monotonie le mieux possible
	* on ne trie pas par ordre alpha mais par frequence
	par ordre ascendant
	* si on fait l'inverse: ordre decroissant
	* si A est freq 100%:
	on sait que A et qqch a de grandes chances de l'etre aussi
	⇒ on se ramene potentiellement a des branches tres profondes
	* par contre si on prend
	pour la branche qui est sensee etre la plus prof possible
	l'item avec la plus basse freq
	* si pas freq → sert a rien de continuer
	* soit deja peu freq → freq va diminuer
	⇒ branches bcp moins prof
	* qq soit l'ordre des items, on aura le meme resultat
	* mais pas forcement le meme nombre d'operations realisees

## eclat enumeration

- on n'en parle pas


## pattern flooding

[](/classes/dcd/preference.based.pattern.mining.006.png)

- on avait fait du minsup avant
- on extrait tous les itemsets de frequence ≥ μ = 2
	* ai: itemsets
	* oi: transactions
⇒ doit apparaitre dans 2 lignes
⇒ tous les sous-ensembles ici sont freq
= 2⁵-1 possibilites pour le premier bloc,
pareil pour les autres → *= 3
si on considere l'ensemble vide (colonne 0) → += 1
= (2² - 1) * 3 + 1 = 94 motifs
- alors que visuellement on verrait 3 ensembles
et eventuellement ∅ de freq 8
- donc en fait 4 motifs interessants,
qui sont finalement suff pour representer les 90 autres
	* 3 fermes + ∅
- on appelle ca une representation condensee
	* si on regarde les diff sous-ensemble,
	tous les motifs sont dans des classes d'equivalence
	* dans une classe d'equivalence
	on a tous les itemsets
	supportes par la meme transaction
	on a vu 2⁵ → 31 elements dans la premiere classe d'equivalence
	* l'enjeu est d'eviter de retourner 31 fois la meme info
	car ultra-redondante
	* on trouve plutot des representants
	pour chacune des classes d'equivalence
- plusieurs types de representants

[](/classes/dcd/preference.based.pattern.mining.007.png)

- unique par classe: on peut prendre l'element maximal = le ferme
- mais on pourrait avoir d'autres types de representants
- generateurs ou libres
	* el minimaux
	* on sait que les fermes sont uniques par classe
	* les minimaux ne le sont pas forcement,
	on peut en avoir plusieurs
- difference entre fermes et maximaux:
	* notion de fermeture ne tient pas compte de la frontiere
	* exemple:

[](/classes/dcd/preference.based.pattern.mining.008.png)

	* on a un espace de recherche en forme de losange parcouru vers le bas
	* on a une frontiere
	* on a des classes a l'interieur avec 1 ferme par classe (triangles)
	* les fermes a la frontiere sont des itemsets maximaux
	car il n'y a pas de surensemble frequent
	* itemset maximal implique qu'il est ferme
	* mais on a des fermes dans des classes d'equivalence superieurs
	qui ne sont pas maximaux
- on peut avoir plusieurs generateurs
	* classes plutot en forme triangulaire /\
	* a la base, plusieurs generateurs
	* qd on les combine vers le sommet
	on a arrive a un seul ferme
- si on se focalise uniquement sur fermes
ou uniquement sur generateurs
	* on aura des sorties plus petites
	* mais aussi des gains relatt importantes en niveau algorithmique
- zB si on se focalise sur la recherche de fermes plutot que les freq,
on peut avoir des gains de plusieurs ordres de grandeur,
genre x1e3
- ou rendre des extractions non-faisables faisables
- si on prend les fermes
→ sauts directs dans l'espace de recherche
et on enumere de ferme en ferme
- enjeu quand on parcourt des espaces de recherche:
qd on regarde un noeud comme AC,
on a plusieurs facons d'y aller
	* on peut passer A ou par C depuis ∅
- l'enjeu est de ne pas parcourir 2 fois le meme element,
car 1 fois de trop
- on veut parcourir au plus un element dans l'espace de recherche
	* on aimerait eviter les non pertinents
- avec la notion de fermeture,
on peut aussi introduire la notion d'ordre canonique
	* on se rend compte qu'on a deja vu qqch
	et on n'a pas besoin de le revoir
- generateurs extremmt faciles aussi
car etre generateur = etre antimonotone
donc on peut utiliser a priori pour extraire les generateurs
- la representation condensee est
la frontiere de la fouille de motifs
avec d'autres domaines
nott des domaines formels issus des maths
comme l'analyse formelle de concepts


## the fim era

- pour illustrer ce qu'on a dit

[](/classes/dcd/preference.based.pattern.mining.009.png)

- pendant des annees on s'interessait qu'aux meilleurs algos
pour les motifs frequents
et algos de plus en plus performants
- on arrive a des graphes comme ca,
comparant des methodes diff
avec des gains de quelques ms
- mais on n'ameliorait pas l'algo du point de vue utilisateur
car qq ms ⇒ nul,
et sorties sont shit
- et donc on est arrive a la notion de contrainte
pour representer les choses avec plus que juste la freq
	* les contraintes permettent a la fois
	de mieux decrire l'interet de l'utilisateur
	et d'elaguer encore plus l'espace de recherche


## pattern constraints: monotone, anti-monotone

[](/classes/dcd/preference.based.pattern.mining.010.png)

- on peut exhiber diff proprietes pour les contraintes
- propriete d'antimonotonie:

[](/classes/dcd/preference.based.pattern.mining.011.png)

	premiere: prop d'anti-monotonie
- si on a 2 motifs phi1 et phi2
et φ1 inclu dans φ2
(= φ1 sous motif de φ2)
	* zB A inclu dans ABC
- pour cette contrainte,
si ABC verifie la contrainte,
alors on peut aussi dire que φ1 la verifie aussi
ABC freq => A freq aussi
- equation: definition
- mais ce qui nous interesse pour les algos c'est la contraposee
- nott si φ1 ne verifie pas la contrainte,
alors φ2 ne peut pas la verifier non plus
	* e.g. ¬C(φ1, D) ⇒ ¬C(φ2, D)
- ca veut dire que quand on fait l'explo,
au lieu de mettre la freq comme contrainte anti-monotone,
on cherche les motifs qui ne contiennent ni a ni c
- motif ac cumule les deux conditions
= viole la contrainte anti-monotone
- si viol, alors tous les sur-ensembles la violent aussi
- donc si on est la, sert a rien de regarder les surensembles
- contrainte tres similaire: monotone
	* φ1 inclu dans φ2
	* mais si φ1 verifie la contrainte
	alors ses supermotifs la verifient aussi
	* zB on veut les motifs contenant b ou c
	* si les motifs b verifient la contrainte,
	tous ses supermotifs aussi
	* pour exploiter les proprietes dans un algo,
	on utilise la contraposee
	* donc si on a un motif qui ne verifie pas la contrainte,
	tous ceux qui sont en dessous ne la verifient pas non plus
- proprietes qui permettent deja de gerer un grand nombre de cas
de facon relatt triviale


## convertible constraints

[](/classes/dcd/preference.based.pattern.mining.012.png)

- supposons que pour tous les items, on a ajoute un prix
e.g. fonction qui dit a cote pour un item son prix
- on peut vouloir cherche tous les itemsets
dont la moyenne de prix est > seuil
- embetant car on ne sait pas trop quoi faire avec la moyenne
	* un motif peut violer la moyenne
	mais on peut ajouter un motif a grand prix
	dont les supermotifs sont bons
	* donc on peut faire augmenter la moyenne
	en ajoutant des choses
	* de la meme facon on peut l'augmenter en supprimant des elements
	* moyenne pas bonne car proche de zero
	→ si on supprime on augmente la moyenne
	⇒ potentielle generalisation:
	un sous motif du motif pas bon devient bon
- shit, la moyenne ne se comporte pas
de facon monotone ou anti-monotone
- mais on peut se ramener a des strategies
ou on arrive a controler l'evolution de la moyenne
- si on trie les items de facon decroissante
et on considere l'ordre dans l'enumeration,
on sait que la moyenne ne peut que diminuer,
car on est parti du plus grand
et on rajoute de plus en plus petits
- cool car on se ramene a qqch d'antimonotone
- zB ac pas bon:
tout ce qu'on pourrait rajouter apres ac
a une moyenne plus petite,
et donc ac* est pas bon
⇒ elague
- une contrainte est convertible
si on arrive a trouver un ordre dans l'enumeration
sur un prefixe
tq si φ1 est avant φ2 dans cet ordre,
on se ramene a une gestion antimonotone de la contrainte
	* on peut avoir aussi une gestion monotone
- notion de convertible:
on peut definir un ordre
grace auquel on peut controler l'evolution
de la mesure sur lequel porte notre contrainte


## contraintes faiblement anti-monotones

- relaxation de la contrainte d'antimonotonie (AM)
- e.g. un motif verifie la contrainte dans les donnees,
alors il existe dans ce motif un element
qu'on peut supprimer
et le motif resultant va aussi verifier la contrainte dans les donnees
- ici exemple avec la variance
- mais autre exemple: certaines notions de graphes denses (pour des graphes)
	*o - o - o
	     \ /
	     / \
	    o - o
	⇒ graphe considere comme dense
	* on peut tres bien supprimer un element
	tq le sous-graphe est aussi considere comme dense
	* suffit de prendre l'element le moins connecte aux autres (*)
- permet de faire des enumerations particulieres
	* on ne les verra pas la
	* on peut se ramener a des enumerations
	ou on a des chemins de solutions
	= on a que des elements de solution sur le chemin,
	et pas de "sauts" avec non-solution puis solution

- exemples de contraintes:

[](/classes/dcd/preference.based.pattern.mining.013.png)

	* v ∈ P: appartenance au motif → M
	* etc


## enumeration strategy


- enumeration tres simple:
enum binaire dans l'espace de recherche

[](/classes/dcd/preference.based.pattern.mining.014.png)

- au depart: ∅ en bas,
en haut: ensemble des literaux (ensemble I)
- enumeration binaire = 2 choix possibles
	* on peut choisir d'enumerer a
	* donc espace de recherche borne par a
	* on peut toujours essayer a partir de la
	d'atteindre abcde
	* donc l'espace de recherche resultant
	une fois qu'on a enumere a
	est le treilli en bleu
	* on peut aussi choisir de ne pas enumerer a
	* donc ne pas ajouter a au motif,
	donc on aura toujours ∅
	et tout en haut on a I \ {a}:
	suppression de a du plus grand motif possible
	⇒ treilli jaune
- on peut toujours representer notre espace de recherche restant
sous forme de treilli
	* motif en cours d'enumeration
	* et en haut, le motif le plus grand qu'on peut esperer obtenir,
	donc P ∪ {candidats}
	* en raisonnant sur les deux bornes,
	on peut rechercher de facon efficace
- formalisation de l'enumeration

[](/classes/dcd/preference.based.pattern.mining.015.png)

	* R∧: motif, R∨: motif + candidats
	* on peut soit choisir pour un element d'ajouter le motif:
	R∧ ∪ {a}
	→ ne change pas le top
	* soit ne pas enumerer
	→ le motif ne change pas,
	mais le top si car on a supprime a

- gestion d'une contrainte M
	* e.g. X ⊆ Y : (on sait que) C(X) ⇒ C(Y)
	* on veut travailler sur la contraposee,
	e.g. ¬C(Y) ⇒ ¬C(X)
	* X = motif actuel (explo depth-first
	* Y = top du treilli, le plus gros motif possible
	* si R∨ pas bon, R∨ forcement pas bon,
	aucune solution viable entre les deux,
	et on peut arreter l'explo
	⇒ ¬C(R∨, D) → empty
- contrainte AM: contraire

[](/classes/dcd/preference.based.pattern.mining.016.png)

	* X ⊆ Y : C(Y) ⇒ C(X)
	* ¬C(X) ⇒ ¬C(Y)
	* ¬C(R∧, D) → empty
	* arret direct et backtrack si depth-first
	* dans a priori, c'est ce qu'on essaie de faire aussi
	meme si c'est en largeur
	* generation de candidats superieurs
	qu'en combinant les frequents ee
	pour eviter de generer des surensembles de motifs nonfreq
- pour les convertibles,
on va juste definir un ordre pour se ramener en AM ou M
	→ fiche de TD


## a new class of constraints

- pour l'instant on n'a vu que des contraintes A ou AM,
mais on peut avoir des contraintes A ou AM par morceau
	* on a des fragments A et d'autres AM
- on va essayer a chaque fois de raisonner comme on a fait

[](/classes/dcd/preference.based.pattern.mining.017.png)

- on peut facilement obtenir les deux extremites du treilli
= motif courant R∧ et le plus grand motif possible (motif + candidats) R∨
- enjeu: essayer de raisonner a chaque fois
pour voir si on a des chances de trouver des solutions entre les deux
- et aussi propager les contraintes
cad elaguer des elements a R∨
- jusqu'a maintenant on a vu
soit on arrete l'explo a R∧ car les extremites ne sont pas bonnes,
mais si on fait de la propagation,
on peut bouger l'extremite (vers le bas),
car si on considere les candidats,
on a des choses qui sont nulles,
et si on les considere on aura des solutions mauvaises,
donc faut les suppr des items candidats
- en gros on bouge le haut du treilli
- de la meme facon on peut bouger le bas
en voyant que pour avoir une bonne solution,
on est oblige d'avoir certains motifs,
et donc on les integre directement
- intuitivement, c'est ce qu'on va faire
- on combine les different types de contraintes M et AM
par morceaux de cette facon,
on cherche dynamiquement a la fois de bouger les 2 bords du treilli
- plus approfondi, et on ne va pas trop en parler


## tight upper bound computation

[](/classes/dcd/preference.based.pattern.mining.018.png)

- juste pour finir, tout ca peut etre vu
comme essayer de calculer des bornes
sur des mesures souvent convexes
- a chaque fois, on va juste utiliser les deux extremites du treilli
pour calculer des bornes qui sont plus ou moins grossieres


## exemple: exo 1 (td)

[](/classes/dcd/preference.based.pattern.mining.019.png)

- ca on sait deja faire
- on va faire juste a priori,
pas les algos d'extraction ferme,
faut juste savoir que ca existe
- modifions l'enonce pour diminuer le nombre de solutions,
mettons minsup = 3 au lieu de 2
- a priori:

	L=1	L=2	L=3
	a:2	bc:5	bce:5
	b:5	be:5
	c:5	ce:5
	d:1
	e:6
	F:1

	* parcours breadth-first
	* on recherche d'abord les items freq,
	on peut retourner les freq de chaque sur 1 passage de la bdd
	* passage a L3: pour generer les candidats:
	combinaison des trucs avec prefixes communs:
	bc, be
	* les items dont on s'est pas servi (ce),
	faut regarder s'ils sont bien frequents,
	sinon bce n'a aucune chance d'etre freq
- definissons les fermes (plusieurs def possibles)
	* x est ferme ssi ¬ ∃ y tq x ⊆ y et supp(x) = supp(y)
	* c'est a dire pour un supp donne, !∃ plus grand motif
	* def qui marche bien pour pas mal de langages
	* pas tout a fait vrai pour des seq
- autre def plus math
	* operateur de derivation → donne l'ensemble des transactions
	qui contiennent X

		X´ → { transactions | X ⊆ t }
			| tq

	* et un autre op de derivation ´
	dans l'autre sens
	qui donne l'ensemble maximal
	donc le plus grand itemset contenu
	dans toutes ces transactions

		∩ { t }
			intersection des t
		X ferme ssi X = X¨
			¨: resultat des 2 operations successives
	* b´ = {bce, abce, bce, abce, bce}
	* b¨ = {bce} (l'intersection)
		donc le ferme pour la classe d'eq ou on a b
		= bce
	* calcul simple de facon lineaire
	* en maths, ces 2 op forment ce qu'on appelle
	connextion de Gallois
	* operateur de fermeture avec de jolies proprietes
	* idempotent: on a besoin de calculer la fermeture qu'une fois

		x¨ = (x¨)¨

			fermeture de x = fermeture de la fermeture de x
			donc rien de nouveau
	* reflexif

		x ⊆ x¨
			x inclu dans sa fermeture

	* additivite

		x ⊆ y ⇒ x¨ ⊆ y¨

	* propriete math d'une correspondance meme de Gallois
	* vrai pour n'importe quel operateur de fermeture
	* exploitable pour elaguer bien plus efficacement l'espace de recherche
	* si bce ferme, bc/be/ce non fermes car inclus dedans
	et ont exactement le meme support
	* b et c inclus dedans et meme support → b et c non fermes
	* supp(e) = 6 ≠ 5, donc ferme aussi
- libres pour generateurs: un peu different
	* x libre ssi !∃ y | y ⊂ x et supp(x) = supp(y)
		y sous ensemble de x
	* motif tq n'existe pas de sousmotif avec le meme support
- on peut representer une classe d'equivalence sous forme d'un treillis /\
	* donc le ferme est bien l'element maximal de la classe d'equivalence
	* donc on impose des sauts avec les supermotifs
	* et les libres/generateurs sont des elements en bas,
	sur la base du triangle
	* pareil, au sens ensembliste sont minimaux
	* et on impose des sauts en termes de support
	avec ceux qui sont dans les autres classes
- pour calculer les libres, faut pas oublier ∅
	⇒ ∅ libre
		{}_6: generateur
	* e a le meme support que ∅, donc ne peut pas etre libre
	* b et c diff → libres
- pour cet exemple la,
on peut donc representer l'espace de recherche
	∅	E
		B
		C
	et on a une classe d'equivalence ou on a {∅, e}, ou e est le ferme
	et une autre classe ou les generateurs sont b et c, et le ferme est bce


## exemple: exo 2 (td)

[](/classes/dcd/preference.based.pattern.mining.020.png)

- 1:

	{ φ ⊆ I | supp(φ,D) ≥ 2 ∧ sum(φ) < 40

- supp(φ,D) ≥ 2 → contrainte AM ⇒ a priori
- sum(φ) < 40: plus bizarre, mais aussi contrainte AM
	* si un motif verifie la contrainte,
	tous ses motifs la verifient aussi
	* et inversement si > 40, tous les supermotifs sont aussi >40
	car prix ≥ 0
- pour l'a priori, on va generer plus intelligeamment
- pour verifier la somme, on n'a besoin que de connaitre les prix,
pas de parcourir les donnees,
donc on integre le calcul de la somme dans la generation
	* cad x_20 et y_30 → on ne genere pas xy
- on fait pareil qu'avant pour les frequences avec les prefixes
- mais on prend en compte la somme dans la generation elle meme
- conjonction de 2 contraintes AM = AM aussi

	L1		L2		L3		L4
	A_10:3		AB_31:2		ACD_37:2	∅
	B_21:3		AC_25:3 	ADF_37:2
	C_15:5		AD_22:2
	D_12:4		AF_25:2
	E_30:2		x AG_32:1
	F_15:3		BC_36:3
	G_22:2		BD_33:2
	x H_101		x BF_36:1
			CD_27:4
			CF_30:3
			CG_37:2
			DF_27:3
			x DG_37:1
			x FG_37:0

- confiance d'une regle
	* regle: expression de la forme
	x,y ⊆ I
	x ∩ y = ∅
	donc quand on a x, on a y aussi
	* pour evaluer l'interet d'une regle,
	on a plein metriques,
	mais nott le support

		supp(R) = supp(x ∪ y)

	et confiance

		supp(R) / supp(x)
		support de la regle / support de la partie gauche
		le % de fois quand on a x,
		on a egalement y
	
	confiance = 1 ⇒ a chaque fois qu'on a x dans une transaction,
	on a y aussi
	cad supp(x ∪ y) = supp(x)
	donc en gros si on dessine la classe d'equivalence,
	ferme = x ∪ y
	et generateur a le meme support
	regle generateur implique ferme prive du generateur (car disjoint)
	a une confiance de 1 car meme support

		R(gen → ferm \ gen) ⇒ confiance(R) = 1
	supp(acd) = 2, supp(ad) = 2
		R: ad → c ⇒ conf(R) = 1
- plus dur:

	{ x ⊆ I | supp(X) ≥ 2 ∧ avg(x) > 21 }

	* avg(x) convertible AM si on considere un ordre sur les items,
	prix decroissant
	* premiere partie AM → a priori
	* 2e partie: notion d'ordre sur le prefixe
	* donc vaudrait mieux faire avec une explo depth-first,
	beaucoup mieux
	* h < e < g < b < c ≤ f ≤ d < a
		(c et f arbitraire)
	* e viole deja la contrainte
	* g* ⇒ pas bon
	* ∀x g<x ⇒ nope
	* mais on peut avoir des solutions contenant g sans que ce soit un prefixe

	∅	- h_101,1 x
		- e_30,2	- eg_26,2	- egb_24.3,2	- egbc_22 x
	  					- egc_22.3 x
				- eb_25.5,2	- ebc x
				- ec_22.5 x
		- g x

		X_u,v: moyenne, support
		egbc pas bon → tout ce qui viendrait apres ne sera pas bon non plus
		⇒ backtrack
		apres egc on a fini d'explorer la branche de prefixe g (eg)
		ebc: nope ⇒ ∀X, ebc < X: aucune chance
		remember: depth first: on va vers la droite
		et on backtrack quand c'est mort
	⇒ done
	⇒ exploite parfaitement dans un parcours depth-first
		bcp moins efficace en breadth-first


## generalization and limits

- tout ce qu'on a vu est illustre sur les langages des itemsets
- mais on peut l'imaginer pour pas mal de choses:
seq, graphes denses, etc
- on a un probleme actuellement:
supposons qu'on est capable de definir notre interet,
on arrive a formalise ce qu'on cherche
	* on trouve un algo qui le fait ⇒ cool
- mais si la contrainte change un peu
	→ on ne peut pas utiliser l'algo,
	faut le modifier nous-meme
	donc limitant
- on a plutot des approches declaratives,
un peu similaires aux bdd
	* requete: sql:
	on ne dit pas comment trouver la solution
	mais on declare ce que l'on recherche
	≠ algebre relationnelle:
	faut mettres les bonnes operations dans le bon ordre
	* en sql on a un optimiseur de requetes qui optimise pour nous
	et calcule la solution
	* on definit ce qu'on cherche
	et la resolution est mise du cote du sgbd
- on peut faire la meme chose:
l'utilisateur declare une intention de ce qu'il veut faire
et la solution est laissee a un solver generique
- plusieurs types de solver
	* cp: constraint programming,
	programmation par contrainte
	* sat solvers, satisfaction,
	solvers tres efficaces,
	se ramenent a chaque fois a des problemes de satisfaction
	* autres types de solvers generiques emergents
	* ilp: integer linear programming
	* actuellement pas efficaces,
	mais doublent leur performance tous les ans
	* asp: answer set programming,
	syntaxe ressemblant a prolog
	et memes defauts que prolog:
	extremement lent
	mais permet d'avoir des approches generiques

## thresholding problem

- la plupart de ce qu'on a vu s'appuie sur des seuils:
freq, moyenne, somme, etc
- tres compliques a mettre en jeu
	* generalement, 2 choses
	* qd contrainte AM,
	* si seuil hyper haut: probleme simple,
	extraction rapide,
	peu de motifs, mais tautologies
	* si seuils super bas,
	explosion combinatoire
	* pareil pour contraintes M
- des approches essaient de contourner le probleme
en utilisant du flou ou des hierarchies
	* zB items = produits
	* coca, pepsi ⇒ soda metaitem
	* on essaye de resumer
	* mais jamais tres satisfaisant comme solution


## notion de fermeture

- extraction efficace de motifs par approche dfs: on s'appuie sur la notion de fermeture
- etant donne un motif X, on a un operateur de derivation ´
qui donne l'ens des transactions de la bd tq X est inclu dans la transaction
 	== toutes les t de D qui verifient le motif X
- autre operateur qui permet d'aller dans l'autre sens,
dans un sens l'intersection de ces t,
donc retourne le plus grand motif qui est verifie par toutes ces transactions
	X ­´→ {t ∈ D, x ⊆ t}
	  ←´­ 
	   ∩tᵢ
- exemple:
	t₁ a b c
	t₂ a b c
	t₃ c
	a´ → {t₁,t₂} ­´→ abc
- operateur de fermeture == application successive de ces deux operateurs de derivation
- le motif X est ferme si la fermeture de X est egale a X
	X ferme si X¨ = X
- dans l'exemple, abc est un motif ferme
- pour rappel, dans l'espace de recherche, on a des classes d'equivalence ({} below),
et un motif ferme est un element max de la classe d'equivalence,
representant unique de cette classe

[](/classes/dcd/preference.based.pattern.mining.021.png)

- la classe d'equivalence capture exactement la meme semantique,
ca identifie tous les motifs verifies par les meme transactions
- on a une redondance d'information la dedans
- au lieu de retourner autant de fois que d'elements qu'ont a la meme information dans la classe,
on se concentre sur un representant, ici ferme
- regle un probleme de sortie et un pb d'enumeration:
on considere ces representants et pas les autres,
donc on enumere bcp moins d'elements dans l'explo


## operateur de fermeture

- cet operateur est connu sous le nom de "correspondance de GALLOIS"
(gallois connection)
- on a des proprietes
	X ⊆ X¨: dans le pire des cas, on aura au minimum X en appliquant l'op
	X¨ = (X¨)¨: idempotence: necessaire une et une seule fois
	X ⊆ Y ⇒ X¨ ⊆ Y¨
- pour chercher des motifs fermes frequents,
dans un dfs,
au lieu d'enumerer a puis ab puis abc etc,
on enumere directement de ferme en ferme
- on enumere X puis on calcule la fermeture,
puis on enumere a partir de cette fermeture ∪ autre element
	X → X¨ → X¨ ∪ {e}
	a → abc → abc ∪ {d}
⇒ sauts dans l'espace de recherche
- plus les classes d'equivalence sont grosses,
plus on gagne du temps,
avec des sauts de plus en plus grands


## test de canonicite

- autre probleme: faut arriver a enumerer
au plus une fois chaque element de l'espace de recherche,
cad les elements peu prometteurs enumeres 0 fois
- on a vu aussi que l'espace de recherche a une structure de treillis
	* on enumere ∅ jusqu'a potentiellement tous les elements du langage
	* mettons qu'on a un point interessant dans l'espace,
	on a plusieurs chemins pour y arriver
	* l'idee c'est qu'on n'enumere ce point qu'une seule fois
- il faut donc prioriser des chemins
- pour ca, on va utiliser un test de canonicite
- on definit un ordre sur les motifs,
par exemple alphabetique/lexicographique
- on utilise l'ordre quand on calcule la fermeture
- pour un motif X on calcule la fermeture
- deux possibilites: la fermeture a permis d'ajouter des elements,
mais le motif resultant est apres X dans l'ordre lexicographique,
donc on continue l'exploration
- par contre si on decouvre un motif avant celui qu'on est en train d'explorer,
ca veut dire que comme on fait un dfs,
on tombe sur une branche qu'on a deja explore
	L pour lexicographique
	X <_L X¨: continue l'exploration
	X >_L X¨: arret de l'exploration


## application: exemple precedent

[](/classes/dcd/preference.based.pattern.mining.006.png)

- assumons un ordre alphabetique: a₁ < a₂ etc
- on peut calculer ∅¨, qui retourne ∅ et le supp du nombre de transactions = 8
(premiere colonne)
- ici assez simple comme on a permute les lignes et colonnes pour avoir de jolis rectangles
- on commence par a₁¨
- a₁¨ supporte par les 3 objets o₁,o₂,o₃
- o₁,o₂,o₃ supportent simultanement a₁,a₂,a₃,a₄,a₅
- donc on peut continuer et la on a differentes techniques d'implementation
- techniques travaillant sur les bases projetees:
plus aucun item a inserer et backtrack
- sinon on essaie d'ajouter un element, zB a₆:
0 transactions le contiennent,
donc on backtrack, etc
- finalement la branche de prefix a₁ est donc terminee
- on passe a la branche a₂: a₂¨ = a₁,a₂,a₃,a₄,a₅
- cette fermeture est avant dans l'ordre lexicographique,
on l'a deja explorer, donc on le barre et on arrete
- pareil avec a₃-₅ → on arrete l'enumeration et on backtrack
- on enumere a₆: a₆¨ = a₆,a₇,a₈,a₉,a₁₀
- si on ajoute un element → rien, on backtrack
- meme fermeture jusqu'a a₁₀,
mais resultat de la fermeture avant dans l'ordre L,
donc on arrete
- donc en exploitant l'operateur de fermeture
et le test de canonicite,
on arrive a des enumerations extremement efficaces
car on peu tres rapidement se rendre compte qu'on a deja enumere un element
et qu'on retombe sur un partie de l'espace de recherche qu'on connait deja
- en pratique, on a des extractions non-faisables avec a priori
qui le deviennent en considerant les fermes,
sinon on peut avoir des gains de l'ordre de 1e3 par exemple


## pattern mining as an optimization problem

[](/classes/dcd/preference.based.pattern.mining.023.png)

- on va faire un shift dans la fouille des motifs,
et voir ca comme un probleme d'optimisation
- jusqu'a present on part du principe que l'utilisateur
est capable de declarer son interet sous forme de contraintes
- en soi, pas si trivial que ca,
sachant qu'en general quand on a des contraintes,
on a des seuils a fixer sur differents parametres,
comme support minimum,
qui necessitent une expertise sur les donnees,
et difficiles a fixer car peuvent engendrer une explosion combinatoire,
donc on aimerait bien se passer de fixer les seuils
- tous les problemes qu'on a fait jusqu'a present,
on peut les voir comme des pb de resolution,
de satisfaction de contraintes,
on cherche toutes les solutions qui satisfont certaines contraintes specifiees
- maintenant on va apprehender le probleme comme un probleme d'optimisation
par rapport a differents criteres:
preferences que l'utilisateur a exprime sur les donnees,
ou en se basant sur la theorie de l'information, etc
- on va moins s'attarder sur les aspects algorithmique
que juste voir ce qu'il est possible de faire
- on va surtout regarder topk,
pour voir ce qu'on peut integrer comme contrainte supplementaire
pour elaguer l'espace de recherche


## addressing pattern mining tasks

[](/classes/dcd/preference.based.pattern.mining.024.png)

- si on s'affranchit des contraintes de seuil,
on peut tres bien emettre des preferences sur des mesures de qualite,
par exemple plus la frequence des motifs est importante,
plus c'est interessant
- on peut aussi utiliser des mesures du domaine,
par exemple pour des molecules comme des toxicophores,
on peut utiliser des connaissances du domaine
comme aromaticite qui depend du nombre de carbones,
et plus c'est important, plus c'est interessant, etc.
- on peut aussi mettre des preferences entre les motifs eux-meme,
par exemple on prefere X₁ a X₂
- motifs statistiquement significatifs etant donne une distribution,
ou ceux qui sont surprenants
- ces preferences permettent de selectionner les motifs les plus interessants
du point de vue de l'utilisateur

[](/classes/dcd/preference.based.pattern.mining.025.png)


## top-k pattern mining

[](/classes/dcd/preference.based.pattern.mining.026.png)

- parmis les approches les plus simples et intuitives,
c'est la recherche des top-k par rapport a une mesure
- par exemple si on regarde les frequents,
on peut regarder les k meilleurs motifs
par rapport a la mesure de frequence
- le probleme de top-k est extremement simple,
notamment pour les frequents
- pour rappel, l'espace de recherche est sous forme d'un treillis,
avec ∅ en haut et l'ensemble de tous les items en bas
- si on montre les k meilleurs base sur la frequence,
du fait que la frequence est une mesure AM/M decroissante en fonction de la specialisation,
on sait qu'en descendant, la frequence va diminuer,
puisqu'on rajoute des items, et si on rajoute un item,
la frequence du motif resultant va forcement diminuer,
c'est une probabilite jointe
	en descendant, on passe de generalisation a specialisation
- donc les top-k sont sur la partie superieure du treillis,
et sont faciles a identifier
- en fonction de comment decroit la frequence en fonction de la specialisation,
on peut avoir une branche plus profonde que les autres,
mais globalement on a en haut les solutions

## top-k pattern mining avec d'autres mesures

[](/classes/dcd/preference.based.pattern.mining.027.png)

- le probleme est plus difficile si on s'interesse a d'autres mesures de qualite
- par exemple on veut les motifs qui maximisent une mesure d'aire
- aire = frequence du motif * cardinalite du motif
	area(X) = freq(X) * |X|
- si on represente nos donnees sous forme d'une matrice,
par exemple dans le tableau sur la diapo on met des croix a la place des lettres
quand l'item apparait dans une transaction,
l'aire du motif est le nombre de croix couvertes par le motif
	freq(BCDE) = 4, |BCDE| = 4
	area(BCDE) = 16
	on retrouve bien 16 elements (4 fois BCDE)
- cette mesure est particuliere,
elle depend de la frequence
qui a tendance a diminuer avec la specialisation,
alors que la cardinalite du motif augmente avec la specialisation
- contrairement aux freq avec les solutions en haut du treillis,
on aura des solutions un peu partout dans le treilli
- plus delicat, car on n'aura pas un chemin continu de solutions pour y acceder


## pruning conditions

[](/classes/dcd/preference.based.pattern.mining.028.png)

- on peut faire un dfs pour chercher les topk par rapport a cette mesure,
mais on peut ajouter des regles d'elaguage supplementaires
- l'idee est qu'on a une liste k de solutions courantes triees,
et on connait a n'importe quel moment quelle est dans cette liste la
quelle est la plus "mauvaise" valeur,
le score du k-ieme motif
- donc on peut s'appuyer sur cette valeur
pour essayer de definir des contraintes
que doivent verifier les motifs qui peuvent esperer entrer dans le topk actuel
- on va utiliser la valeur A pour dire
que c'est la plus petite valeur du topk actuel
- on sait que dans les transactions,
la taille de la plus grande transaction est de 6
- a partir de la, on peut definir un propriete
qui dit que pour qu'un motif X puisse etre inserer dans l'ensemble des topk actuels,
il faut qu'il batte ce plus mauvais topk
- pour le battre, il faut qu'il ait une frequence > A/(taille de la plus grande transaction)
	freq(X) ≥ A/L
- ce qui est interessant qu'on a un support qui depend de A/L,
mais au fur et a mesure de l'enumeration, 
le A est amene a augmenter,
car plus on enumere, plus le k-ieme devra etre bon
- par exemple si on commence a enumerer avec {B,BE,BEC},
le plus mauvait a une aire de 6
	B present 6 fois, avec une cardinalite de 1
- donc la frequence a battre est 6/6,
donc on veut les motifs qui sont presents dans au moins une transaction
- en continuant, a la place de B on rajoute BCDE
- le plus mauvais motif dans {BE,BEC,BCDE};
la le plus mauvais motif a une aire de 10,
ce qui fait qu'on a un seuil a 10/6,
plus stringent
- plus on continue a enumerer,
on met a jour la liste des topk
et plus le seuil devient important
et permet d'elaguer une grande partie de l'espace de recherche
- avec les topk on peut definir des strategies de ce type
ou des qu'on a une liste de topk remplie,
on regarde la valeur du plus mauvais,
et on peut ainsi deriver des proprietes
que doivent respecter les motifs qu'on recherche
pour rentrer dans la liste des topk
- pour l'initialisation, si on fait un dfs,
on peut prendre les k premiers elements dans le parcours


## top-k: conclusion

[](/classes/dcd/preference.based.pattern.mining.029.png)

- on a plusieurs avantages,
on cherche les k meilleurs sur une mesure
- pas besoin de definir un seuil sur la mesure a exploiter,
juste a definir la valeur k
(on peut critiquer ou pas l'usage de ce k)
- permet de trouver directement les meilleurs motifs
- on a toujours des cas ou chercher les topk
dans le probleme general reste NP-complet
- les approches completes peuvent donc ne pas marcher
- alternatives avec des heuristiques
	* beam search, recherche en faisceaux
- on a aussi un probleme de diversite,
car generalement etant donne une mesure qu'on cherche a optimiser,
quand on trouve la meilleure solution,
les 2e, 3e, 4e etc solutions sont des variantes de la 1ere,
donc a l'arrivee on identifie une zone tres dense de l'espace de recherche
avec toutes ces variations
- donc on aura le meilleur motif, puis des motifs tres similaires
- on peut essayer de regler ce probleme avec des solutions ad-hoc,
mais pas forcement toujours satisfaisant
- de plus, topk est tres bien sur papier quand on connait la mesure qu'on cherche a expoiter,
mais si on cherche a exploiter simultanement sur plusieurs mesures,
comment on fait?
	* zB topk par rapport a aire ET frequence: on sait pas faire
- pour traiter plusieurs mesures simultanement, il faut d'autres paradigmes
	* zB skyline patterns, fronts(???) de pareto
	sur l'espace des motifs en fonction des preferences


## skypatterns: pareto dominance

[](/classes/dcd/preference.based.pattern.mining.030.png)

- on parle de skypatterns car en bdd,
on a un operateur skyline qu'on a voulu etendre sur des motifs:
calculer la skyline sur l'espace de motifs
- c'est un operateur simple qu'on a surement deja fait
- imaginons qu'on part en vacances en mer et qu'on cherche un hotel,
pour lequel on a des criteres de preference:
on veut minimiser le prix, la distance a la plage,
et eventuellement d'autres
- a distance egale de la mer, on prend le moins cher:
on dira qu'il domine les autres sur ce critere la
- si on represente l'espace des donnees avec distance en abscisse et prix en ordonnee,
on dira qu'un point en bas et a gauche
domine ses confreres plus a droite sur l'abscisse en distance,
mais aussi ses confreres plus en haut pour une distance donnee,
et egalement tout ce qui est entre (plus haut, plus a droite),
par rapport a la distance ET au prix,
donc domine tout seul:
on n'a pas trouve plus proche et moins cher
- on peut avoir des points plus a gauche OU plus en bas:
des hotels uniquement moins cher mais plus loin,
ou proches mais plus chers
- la skyline est ainsi le front de pareto
de tous ces points non domine par les autres,
donc les plus proches de l'origine
- skyline facile a calcule
car une fois qu'on a compris la notion de points incomparables,
non domines par les autres,
c'est juste une double boucle dans la bdd
pour chercher si pour un point d'autres le dominent
- "simple" dans le cadre des bdd,
mais plus complique pour les motifs,
car double boucle sur l'espace de recherche est du exponentiel
- supposons qu'on veut calculer une skyline d'un motif
par rapport a l'aire et la frequence,
on veut les motifs qui maximisent les deux
- sur l'exemple, on a mis tous les motifs possibles,
et sur la skyline, on a 4 motifs: BCDE, BCD, B et E
- n'importe quel autre est domine par 1 de ces 4,
par exemple BCE a la meme frequence que BCDE (4),
mais une aire moindre (12)
- avec les 2 preferences emies par l'utilisateur,
on peut directement retourner ces 4 motifs


## skylines (bdd) vs skypatterns (mining)

[](/classes/dcd/preference.based.pattern.mining.032.png)

- en bdd la tache est tres simple,
car l'espace de recherche est les tuples (transactions),
donc chercher la skyline avec un algo naif sera en O(|D|²)
- mais pour les motifs,
un algo naif sera en O(|L|²),
et ca c'est souvent exponentiel
	|L|: taille du langage
- donc on ne va clairement pas faire une double boucle sur l'espace de recherche


## skylineability

[](/classes/dcd/preference.based.pattern.mining.033.png)

- pour resoudre le probleme sur les motifs,
intuitivement on va etudier toutes les mesures
sur lesquelles portent les preferences
et identifier un sous-ensemble de mesures sur lesquelles on peut travailler,
et directement deriver des resultats sur les autres
- on peut calculer directement une representation condensee
par rapport a ces mesures,
par exemple on s'interesse aux fermes ou aux generateurs
- on n'a pas d'exemples sur les slides,
mais si on s'interesse a la frequence et l'aire,
on peut dire que freq skylineable
- cad si on travaille sur la frequence,
on peut calculer les fermes sur la frequence
- et on sait qu'avec ca,
on peut trouver la skyline sans directement regarder l'aire
- quand on est dans une classe d'equivalence,
on a un ferme en haut,
et tous les elements de la classe d'equivalence ont la meme frequence
- si on prend le ferme et un autre element dans la classe,
on sait que le nombre d'elements dans X sera ≤ nombre d'elements dans le ferme,
puisque le ferme est l'element maximal dans la classe d'equivalence,
c'est a dire celui qui a la plus grosse cardinalite
	|X| ≤ |X¨|
- donc a frequence egale, si on a une plus grosse cardinalite,
on aura forcement une aire plus importante,
donc on sait que le ferme dominer tous les autres elements de sa classe d'equivalence
pour la frequence et l'aire sur ces deux preferences la
- dans d'autres cas il faudra s'interesser aux elements minimaux des classes d'equivalence

[](/classes/dcd/preference.based.pattern.mining.034.png)

- sur l'exemple, on cherche a maximiser sur l'aire et la frequence
- on sait que tout ce qu'il y a a gauche et en bas de BCDEF est inutile,
0 espoir,
ce qu'on va representer par une contrainte
- pour pouvoir esperer etre dans la skyline,
il faut ne pas etre domine par BCDEF (donc dans la partie grisee)
- donc maintenant ce qu'on recherche c'est les motifs
qui sont fermes sur la frequence
ET non domines par BCDEF

[](/classes/dcd/preference.based.pattern.mining.035.png)

- en continuant, avec BEF on augmente la zone de domination,
ce qu'on modelise par une nouvelle contrainte,
et ainsi de suite

[](/classes/dcd/preference.based.pattern.mining.036.png)

- pour conclure, pour chercher des motifs skyline (skypatterns),
il faut etudier des mesures sur lesquelles portent nos preferences,
voire sur quelles sousensemble de mesures on peut calculer nos representations condensees,
donc soit des fermes, soit des generateurs minimaux selon ce qu'on recherche
- et a partir de la on peut faire de facon dynamique en integrant de nouvelles contraintes
- note/nomenclature: les skypatterns composent une skyline


## soft patterns and other approaches

[](/classes/dcd/preference.based.pattern.mining.037.png)

- on peut utiliser des approches plus floues sur la relation dominante
- on ne va pas developper plus

[](/classes/dcd/preference.based.pattern.mining.038.png)

- on va plutot regarder d'autres approches,
comme travailler sur des notions de trouver les bons ensembles qu'on va retourner en sortie
- jusqu'a present, on enumere des solutions
sans forcement s'interesser a l'ensemble des solutions
- on s'y interesse un peu quand on fait des fermes,
puisqu'on dit que pour tous ceux qui capturent la meme semantique, on rend l'element maximum
- aussi avec les skypattern, on cherche ceux qui ne sont pas domines par les autres
- mais plus generalement on peut definir ca comme un probleme de patternset mining
- on cherche a extraire un ensemble de motifs qui verifie des bonnes conditions
- le probleme est qu'on rajoute une complexite supplementaire au O(|L|²) de tantot,
car l'ensemble de tous les ensemble de motifs est 2^(taille du langage),
une puissance de quelque chose qui est deja exponentiel
- malgre tout, on ne va pas chercher avec des approches correctes et completes,
mais par rapport a des heuristiques
- on a plein de taches ou il faut extraire un ensemble de motifs
qui verifient des bonnes conditions
- par exemple en classification
	* on a des regles d'association ou motifs discriminants
	* on cherche le plus petit ensemble de regles
	qui permet de construire un bon modele sur les donnees
	a des fins de prediction
- tout ce qui est tiling (pavage):
essayer resumer les donnees avec le moins de motifs possible
	* zB en revenant sur la representation en matrice binaire,
	c'est couvrir tous les 1 dans la matrice avec le moins de motifs possible,
	donc identifier des motifs qui idealement se chevauchent le moins possible,
	etc
- autre exemple: simplification de formules logiques


## minimum description length

[](/classes/dcd/preference.based.pattern.mining.039.png)

- on peut aussi identifier ces ensembles de motifs
par des approches theorie de l'information
- on dira qu'un bon ensemble de motifs est celui qui compresse bien les donnees
- on va utiliser un dictionnaire
	D: bdd
	CT: ensemble de motifs
- on utilise les motifs CT pour compresser la base de donnees,
donc la resumer avec un autre code
- notion de code table qui donne pour un motif son code
- intuitivement, plus le motif couvre les donnees,
plus il aura un petit code,
donc on compressera bien les donnees en termes d'information
- tres similaire a l'encodage de huffman
- association d'un code en fonction de la frequence de l'element dans les donnees:
plus il est frequent, plus son code est petit
- l'enjeu est de trouver des motifs avec leur code associe
pour essayer de minimiser L(D,CT)
	L(D,CT) = L(D|CT) + L(CT|D)
	L(D|CT): taille des donnees reecrite en fonction du codage
	L(CT|D): taille du code table
- on appelle ca le minimum description length:
trouver l'ens de motifs tq on recode les donnees de la facon la plus petite possible
- on peut le faire dans plein de cas d'usage
- par exemple imaginons qu'on est une chaine de supermarches
et qu'on a plein de diff supermarches a l'echelle nationale
- on peut voir la norme est toutes les choses communes vendues entre tous les supermarches,
donc motifs qu'on code tres bien,
- et on a aussi les differences: tous les motifs tres bons pour certains supermarches,
et beaucoup moins ailleurs
- on peut ainsi voir quels sont les achats qui sont populaires a l'echelle nationale,
et ceux qui sont propres a certaines regions
- d'autres adaptations permettront de trouver des causalites
	* quand interprete des regles,
	on les interprete souvent a tort en termes de causalite,
	alors que les regles d'association etc sont juste de la co-occurrence
	* mais on peut faire certaines formes de causalite
- on peut aussi faire dans les streams (flux) de donnees,
on peut faire du "concept drift detection",
cad trouver dans le flux des changements de distribution de comportements
	* les encodages qui marchent super bien,
	quand on a une fenetre glissante sur le flux,
	ca code tres bien les donnees,
	et tout d'un coup le codage echoue,
	et la c'est qu'il se passe quelque chose de nouveau dans le flux
	* utilisation sur des donnees twitter, etc
- c'est une facon aussi de traiter les donnees manquantes,
pour comprendre si c'est qu'il n'y a pas d'information a mettre,
ou si c'est que l'information a ete omise,
et si c'est le deuxieme cas, quelle information il faudrait qu'il y ait
	* on peut l'apprehender avec des mdl,
	on voyant que ca se compresse super bien
	s'il y avait eu cette information
- approche qui commence a bien se repandre,
et des outils et packages apparaissent se basant sur des approches a minimum descriptionnel,
sur des graphes, etc
- le probleme c'est que ca marche qu'avec la notion de frequence,
car a chaque fois on essaie de trouver le code optimal pour des motifs
en fonction de leur apparition
- d'autres mesures plus sophistiquees
ou qui doivent prendre en compte la connaissance de l'utilisateur ou du domaine,
c'est impossible ici, mais peut etre combine a d'autres approches


## subjective interestingness: interet subjectif

[](/classes/dcd/preference.based.pattern.mining.040.png)

- autre type de probleme interessant, joli sur papier
- on part des donnees et de l'utilisateur qui a des connaissances a priori dessus,
a minima 2-3 requetes sql sur la base pour connaitre le nb d'objets d'un type etc
- par exemple imaginons qu'on a une ville decoupee en quartiers,
et pour chaque tous les commerces, leur type et leurs emplacements
- on peut dire que l'utilisateur connait le nombre total de bars sur la ville,
et l'importance d'un quartier donne en termes de commerce
- on va representer ces connaissances sous forme de "equality constraints",
cad l'incertitude de l'utilisateur sur les donnees, zB valeur moyenne
- maintenant il faut arriver a prendre un modele qui verifie ces contraintes
- cependant on peut en avoir une infinite
	* par exemple il y a une infinite de modeles qui maximisent la probabilite d'une temperature
	* on peut avoir une normale centree sur la temperature,
	on peut aussi avoir une loi uniforme sur cette valeur
- le modele qui verifie les contraintes sans faire d'hypotheses supplementaires
s'appelle un modele a entropie maximale (maximal entropy model),
qui se clacule assez bien
- maintenant des qu'on a un motif,
on peut evaluer son interet par rapport au modele,
on mesure a quel point le motif est suprenant
par rapport aux connaissances de l'utilisateur
- on appelle ca l'information content,
information capturee par le motif
- imaginons qu'on montre un motif interessant a l'utilisateur,
l'utilisateur met a jour ses connaissances,
et de la meme facon on met a jour le modele
en inserant de nouvelles contraintes
- on peut trier nos motifs par rapport a l'information qu'ils capturent
- plus un motif est long, plus la condition est longue et plus c'est difficile a assimiler
	* motif avec 1 seul item, plus "simple" a expliquer
	qu'avec 50 items
- on peut definir ainsi un cout d'assimilation
d'un motif qu'on definit comme la taille de la description necessaire pour coder le motif,
ce qui est discutable
- donc l'interet subjectif c'est essayer de resoudre
le compromis entre information capturee (IC)
et le cout d'assimilation (DL)
	SI = IC/DL
	DL: description length
- donc on cherche les motifs les plus simples
qui capturent le plus d'informations
- ce qui est interessant est que dans le framework general,
on cherche a chaque fois le meilleur motif,
cad qui maximise le ratio IC/DL
- on le retourne a l'utilisateur qui en prend connaissance,
le valide, met a jour ses croyances
et on met a jour nos connaissances
- par exemple tres forte concentration de bars a un endroit,
et l'utilisateur ne le savait pas
- on peut ainsi chercher iterativement les meilleurs motifs par rapport a ca
- par rapport aux topk, ou generalement on a le meilleur cas
et des variations de celui-ci,
la en mettant a jour le modele,
un motif tres similaire au premier sera attendu par l'utilisateur,
donc sera declasse a l'iteration suivante
	* par exemple 2e motif a la 1ere iteration se retrouverait 40e,
	car en mettant a jour le modele, il est considere comme attendu,
	par rapport a d'autres motifs qui seront promus
- on peut travailler a la fois sur la partie IC,
dans le sens ou il faut arriver a representer le plus possible
les connaissances a priori de l'utilisateur,
	* on a la des travaux ou on considere que l'utilisateur connait les marges,
	en gros on fait des 'select *',
	mais on peut avoir des choses plus sophistiquees,
	par exemple des hierarchies avec des valeurs differentes
		. zB nombre de restorants a Londres,
		puis specialisations suivant le type de cuisine
		. on peut analyser differentes villes comme ca
		. etant londonien, qu'est-ce qui est nouveau pour moi si j'analyse amsterdam?
		. connaissance plus structuree, sur une hierarchie,
		ou on propage de l'information par specialisation
- on peut aussi travailler sur l'aspect descriptionnel,
notamment quand on travaille sur des langages plus sophistiques que de simples itemsets
	* par exemple sur des graphes: imaginons qu'on a un graphe tres connexe
	* on le coderait en listant tous ses sommets
	* mais on peut le representer de facon alternative, plus intuitivement
	* finalement equivalent a tous les sommets a une distance ≤ 1 de S₀, le centre
	* pour le coder on a besoin du centre et de la notion de rayon,
	et eventuellement stoquer des erreurs,
	si un sommet n'est pas dans le motif (sous-graphe)
	* donc on peut chercher a optimiser la DL d'un motif,
	pour voir qu'il a un cout d'assimilation moindre sous une autre representation
- dire que le coup d'assimilation est fonction de la taille du codage
est une vue d'informaticien,
et des travaux ont ete fait en sciences humaines,
ou on peut avoir des notions d'acceptabilites qui rentrent en compte,
et on a des domaines ou plus on met des conditions,
plus ils font confiance a la regle,
c'est a dire une regle avec une premice tres riche,
car en termes d'acceptabilite, ca fait "plus serieux", etc
- donc il faut faire attention en pratique,
car d'autres notions qu'on ne maitrise pas toujours peuvent entrer en jeu


## conclusion: problemes d'optimisation

[](/classes/dcd/preference.based.pattern.mining.041.png)

- on fait quand meme une hypothese forte
que l'utilisateur est capable d'exprimer son interet sous forme de preferences
- des fois on a plutot des intuitions mais sans qu'on puisse les formaliser
- donc maintenant on va essayer de trouver des motifs automatiquement
sans connaitre ce que l'utilisateur recherche


## interative pattern mining

[](/classes/dcd/preference.based.pattern.mining.042.png)

### overview

[](/classes/dcd/preference.based.pattern.mining.043.png)

- double tache: on veut extraire des motifs,
mais aussi construire/apprendre un modele de preference pour l'utilisateur
puis l'integrer pour trouver les motifs qui maximisent ces preferences
- pour ca on a une boucle interactive,
on extrait des motifs,
on les presente a l'utilisateur,
qui emet des retours dessus,
qu'on integre pour apprendre les preferences de l'utilisateur
- plus on a une representation fine des preferences de l'utilisateur,
mieux on arrive a extraire des motifs pertinents
- la partie interaction peut etre un score qualitatif like/dislike,
ou plus sophistique comme ordonner les motifs en fonction d'une preference,
ou donner des notes a chaque motif
- dans l'etape d'apprentissage,
on essaie de generaliser les retours de l'utilisateur
pour essayer de construire un modele de preference sur lui


### bonn click mining

[](/classes/dcd/preference.based.pattern.mining.044.png)

- exemple d'outil existant
- extraction simultanee de motifs en fonction de plusieurs mesures d'interet,
puis en fonction des retours, priviligie tel ou tel motif


### global challenges

[](/classes/dcd/preference.based.pattern.mining.045.png)

- problemes associes a chaque etape
- fouille: qui dit interactif, dit instantane
	* motifs frequents: NP-complets
	* meme si on a vu des algorithmes corrects et complets,
	on n'a aucune garantie en temps
	* on veut ne retourner qu'un seul motif de facon instantanee
- il faut aussi extraire des motifs en prenant en compte les preferences,
donc pas agnostic par rapport aux retours de l'utilisateur
- il faut assurer une certaine diversite dans les motifs proposes
	* important pour l'usage: cote utilisateur,
	toujours mieux d'avoir des resultats divers,
	sinon impression qu'il y a trop de redondance dans les donnees
	* pour construire les modeles de preference,
	il vaut mieux diversifier pour "fermer des portes",
	cad affiner le modele de preference
	meme en proposant des motifs qu'on sait etre pas interessants
		. c'est comme si on parcourait un espace de recherche different
		sur les preferences de l'utilisateur
		. si on demande des retours toujours sur les memes parties,
		on n'arrivera pas bien a generaliser
- interaction
	* retours extremement simple de l'utilisateur,
	wrt boulot pour l'utilisateur
	* mais plus on demande des choses a l'utilisateur,
	plus on peut l'exploiter dans l'apprentissage
	* il faut veiller a conserver l'engagement a l'utilisateur,
	sinon il en aura tres vite marre
	* donc compromis moindre effort et retours maximaux
- apprentissage
	* bien construire les preferences
	* bien exploiter les preferences dans le processus de fouille
	* parfois c'est bien d'avoir des preferences interpretables,
	car elles seront probablement transformees
	en contraintes synctactiques ou similaire,
	plutot que quelque chose qui capture les preferences tres bien,
	mais qui est une boite noire
⇒ optimal mining problem (according to preference model)
+ active learning model


### learning: preference model

- il faut etre "patient": bien generaliser les preferences
- le modele construit depend vraiment de ce qu'on presente a l'utilisateur pour feedback
- il ne faut pas toujours presenter les "meilleurs" motifs,
car on risque de toujours rester dans la meme zone de l'espace de recherche,
et avoir du mal a generaliser


#### weighted product model

[](/classes/dcd/preference.based.pattern.mining.046.png)

- on a differents modeles de preference,
dont des tres simples qui s'appuient sur des scores
associes a chaque item
- l'interet d'un motif par rapport aux preferences est le produit des scores
- dans l'exemple, on prefere AB a BC, car il a un score superieur


#### feature space model

[](/classes/dcd/preference.based.pattern.mining.047.png)

- des modeles plus sophistiques pour presenter les preferences existent
- on sait par exemple que le motif A domine le motif C ou B
- chacun des motifs peut etre represente via un vecteur de descripteurs
dans un autre espace
- il faut arriver a trouver ce mapping,
et apprendre dans le nouvel espace
la separation entre les "bons" a ceux que l'usr ne prefere pas

[](/classes/dcd/preference.based.pattern.mining.048.png)

- hypothese qui est faite: on peut capturer les prefs de l'utilisateur
dans ce nouvel espace
- en pratique, on a comme descripteurs les attr du jeu de donnees,
les items: les items appartiennent a l'itemset, etc
- on a aussi des valeurs de freq attendu/observe:
frequence reelle observee dans les donnees,
et, connaissant la distribution sur les donnees,
la frequence attendue
- apres quelques retours on peut par ces approches assez rapidement
si l'usr prefere des choses surprenantes ou attendues
- on a d'autres mesures: cardinalite, scores χ², etc
- dans les tous premiers travaux, l'espace des descripteurs etait tres simple
- la tendance est maintenant plus on a de descripteurs, mieux c'est


### interact: user feedback

[](/classes/dcd/preference.based.pattern.mining.049.png)

- comme dit tout a l'heure,
il faut toujours arriver a ne pas fatiguer l'utilisateur
- type de retours le plus simple dans la mesure du possible
- evidemment, modele plus sophistique → on a besoin de plus de choses de l'usr


#### weighted product model

- like/dislike

[](/classes/dcd/preference.based.pattern.mining.051.png)

- on veut maintenant utiliser le feedback pour apprendre des preferences
- dans le weighted product, on associe un score a chaque item
et le score global de l'itemset est le produit
- comme sur la diapo, on va mettre un 1 quand l'item appartient
a un motif aime par l'usr,
et -1 sinon
- le score pour un item est ainsi 2^(likes - dislikes)
- donc scores >> 1 pour items appartenant a des motifs apprecies
- et scores < 1 pour l'inverse
- marche tres bien pour ce modele qui reste extremement simple



#### feature space model

[](/classes/dcd/preference.based.pattern.mining.050.png)

- on ordonne les motifs par rapport a des preferences
- on peut aussi donner des notes

[](/classes/dcd/preference.based.pattern.mining.052.png)

- pour l'apprentissage on est ici dans le cadre
de problemes de type learning to rank
- on prend une paire d'objets quelconques,
et on voit si le premier element est prefere au second, etc
- donc on apprend des separations entre paires d'objets
- l'ensemble d'apprentissage va etre des differences entre 2 motifs
- on essaie d'apprendre une separation entre objets qui dominent et objets domines
- l'algo le plus utilise: SVM rank, adaptation des SVM
(support vector machines, machine a vaste marge)
- supposons qu'on a des objets d'une classe d'un cote et d'autres objets de l'autre,
mettons des triangles et des ronds
- on va apprendre un modele qui separe les triangles et les ronds,
et on pourra classifier de nouveaux objets avec
- infinite de separations possibles, et faut trouver une bonne solution dedans
	* dans un exemple simple avec deux paquets bien separes,
	c'est en general juste l'hyperplan
- dans un plan en 2d avec 2 classes a separer,
on veut une droite qui est aussi loin de l'une que de l'autre classe,
ce qui permet de mieux generaliser les donnees,
et c'est l'objectif des svm
- svm rank va faire pareil, mais au lieu d'avoir des cercles et des triangles,
on aura des paires
	pour un probleme o₁,o₂ on veut savoir lequel domine
	(o₁,o₂) ⇒ o₁ > o₂
	en utilisant les differences de features
	par exemple, la valeur de freq observee est extremement differente de l'attendue
	et l'autre beaucoup moins:
	peut etre que l'usr prefere des choses tres surprenantes
	et la frontiere se fait la dessus
- le learning to rank problem est bien plus large que cette application
et est retrouve partout, par exemple moteur de recherche
	* on ordonne les reponses d'une requete en fonction des preferences
	* peut-etre qu'on connait une preference pour telle ou telle chose,
	on la mettra en avant
	
#### active learning problem

[](/classes/dcd/preference.based.pattern.mining.053.png)

- autre probleme: quels sont les objets qu'on montre a l'usr?
- on peut avoir des choses qu'ont connait comme pas bonnes,
mais on veut generaliser
- apprendre les preferences de l'usr est un probleme de generalisation,
donc on ne doit pas toujours rester au meme endroit
- on parle de probleme d'active learning:
on est dans un processus d'interaction,
donc on veut construire un modele de pref,
mais tout en essayant de faire le moins de boucles d'interaction possible
et donc le plus rapidement que possible

#### heuristiques

- differentes heuristiques existent pour trouver les bons objets a presenter
- pour bien generaliser, on a besoin de diversite
- heuristiques de diversite locale:
on a des choses relativement differentes dans ce qu'on presente a l'usr
- heuristiques de diversite globale:
on tient compte de cette diversite dans l'ensemble de la boucle,
et pas qu'a une seule etape


##### formalisations mathematiques

[](/classes/dcd/preference.based.pattern.mining.054.png)

- maximal marginal relevance:
qualite intrinseque du motif par rapport au modele de pref
+ parametre de diversite
	* on regarde ici a quel point le motif est different des autres qu'on proposerait a l'usr
	* on propose donc des motifs interessants et bien differents entre eux,
	qui maximisent ce critere la
- global mmr: on peut faire la meme chose
mais en prenant la diversite sur l'ensemble des boucles d'interaction,
et pas que a l'instant t
- la troisieme heuristique s'appuie aussi sur le fait
que generalement l'usr prefere ce qui se passe dans des zones denses des donnees,
donc favoriser un facteur de densite,
en disant qu'un motif represente une zone relativement interessante,
et qu'il faut qu'il soit booste


### mine: mining strategies


#### post-processing, optimal pattern mining

[](/classes/dcd/preference.based.pattern.mining.055.png)
[](/classes/dcd/preference.based.pattern.mining.056.png)

- il faut produire des resultats divers et de facon instantanee,
en incluant les prefs
- dans les premiers travaux, on triche un peu,
on extrait d'abord tous les motifs puis ne fait juste
que retrier les motifs deja trouves,
pas de fouille instantanee
- diversite → approches completes, c'est mort, trop lent
- on peut dire qu'on a un budjet temps, par exemple 2sec:
meilleurs motifs retrouves en 2sec
- soit en dfs, mais on n'aura pas de diversite,
car on risque d'explorer que quelques branches
et tous les motifs vont se ressembler
- soit en bfs, mais que des motifs de 1er niveau,
pas forcement interessants
- donc il faut d'autres strategies


#### pattern sampling

[](/classes/dcd/preference.based.pattern.mining.057.png)
[](/classes/dcd/preference.based.pattern.mining.058.png)

- on oublie la completude, et on fait de l'echantillonnage
- on connait ca pour les bdd, beaucoup de travaux dessus
- on travaille sur un sous ensemble representatif de tuples
- tout ce qui est a base de reservoirs etc,
on a une garantie sur la selection
d'avoir une approximation avec une erreur bornee
- nous ca nous aide clairement pas
- on a vu que les algo sont polynomiaux par rapport a la taille des donnee,
donc si on prend un sous-ensemble, on va ameliorer
- mais le vrai critere de difficulte est la taille de l'espace de recherche
- en selectionnant un sous ensemble de tuples,
on ne modifie pas l'espace de recherche,
on a juste l'operation de calcul du support plus rapide,
car un pass sur la bdd est plus rapide
- donc meme nombre d'items dans l'echantillon,
donc combinatoire de 2^(Nitems)
- ce qu'on veut c'est echantillonner directement l'espace des motifs
- "output space sampling", echantillonnage de la sortie


##### output space sampling

[](/classes/dcd/preference.based.pattern.mining.059.png)

- avec l'echantillonnage,
on n'est plus dans des notions d'optimalite,
on est des choses representatives,
mais aucune garantie d'avoir les top
- on a plein d'approches differentes
- les premiers travaux faits sur l'echantillonnage
sont apparus pour les graphes,
un domaine de motifs assez compliques,
et pour des mesures autres que la simple frequence
- alors qu'on a vu que dans la fouille de motifs,
on est partis de choses tres simples
en complexifiant progressivement les domaines de motifs
- ici on part du principe que les algos corrects et complets
ne sont pas suffisants pour ces domaines de motifs compliques,
donc faut directement faire de l'echantillonnage


##### approche naive

[](/classes/dcd/preference.based.pattern.mining.060.png)

- extraire tous les motifs possible et les echantillonner directement
- aucun interet, etape exhaustive prohibitive
- on veut directement echantillonner l'espace de recherche
sans passer par la fouille
- on a une etape de pretraitement
pour identifier un tuple dans les donnees,
pour directement arriver a la fin
- meme si on fait le pretraitement qu'une fois,
il doit rester faisable
- puis l'etape d'apres doit etre hyper rapide


##### deux familles d'approches

[](/classes/dcd/preference.based.pattern.mining.061.png)

- stochastique: peu de pretraitement, quasi-nul
	* metropolis-hastings
	* monte carlo markov chains
- on fait des marches aleatoires dans l'espace de recherche,
et au fur et a mesure du random walk, on sort des motifs
- il faut evidemment avoir quelques belles proprietes theoriques
pour avoir une garantie que chaque motif qui peut etre tire
a une probabilite non-nulle, etc

- deuxieme possibilite: echantillonnage direct
- on a un pretraitement qui affecte une probabilite d'etre tire
a chaque transaction de la bdd
- et une fois qu'on tire cette transaction,
on peut generer un itemset
- "two-step random procedure"


##### two-step procedure example

[](/classes/dcd/preference.based.pattern.mining.062.png)

- supposons qu'on fait l'approche complete,
on aura l'ensemble en haut de tous les motifs possibles,
7 motifs, 14 transactions (somme des freq)
- a partir de la,
si on a un echantillonnage qui tient la route
et qui prend bien en compte la distribution des motifs freq,
si on demande de tirer 14 motifs,
on aura les itemsets avec les memes frequences
- mais en pratique, pas possible,
donc on veut completement shunter la premiere etape

[](/classes/dcd/preference.based.pattern.mining.063.png)

- on va plutot faire une etape de pretraitement
qui va nous donner un poids pour chaque transaction
	* t₁: 3 items, 2³ - 1 motifs si on ne prend pas ∅
	* on peut utiliser le nombre de motifs contenus dans la transaction comme poids
	pour faire un tirage ensuite
	* t₁ contient 7 motifs, les autres 7 aussi au total,
	donc 1/2 chances de tirer t₁
	* 3/14 chances de tirer t₂, etc
- on est capable maintenant de tirer une transaction
en respectant les probabilites affectees
- puis on choisit de facon uniforme un motif contenu
	* ie. chacun des 7 motifs de t₁ aurait une probabilite de 1/7
- evidemment on ne va pas generer tous les motifs d'une transaction,
car c'est exponentiel
- on va faire autrement
	* l'alphabet des 7 motifs est {A,B,C}
	* on va donner une probabilite de 1/2 a chacun,
	et faire un tirage pour chaque dans un boucle
		A B C
		1 0 1 ⇒ AC
- ce type d'approche permet de tirer des motifs frequents
de facon instantanee
	⇒ le tp1 etait sense etre l'implementation de la methode en python
	en suivant le papier original dessus
	⇒ dcd/tp0 dir has paper + old tp questions

- marrant car si on fait un algo correct et complet,
il faut faire quelque chose de complexe avec du memory management, etc,
alors qu'ici c'est 4-5 lignes de code et des resultats instantanes
- par contre tester l'implementation est plus delicat
- pour un algo correct/complet, on peut faire un toy example et tourner dessus
- ici on a de l'aleatoire et c'est plus compliquer a debug,
il faut reflechir a des strategies pour verifier que la sortie est correcte
- la on a de belles proprietes theoriques,
car un motif va etre tire proportionnellement a sa frequence dans les donnees,
donc il faut par exemple verifier cette propriete, etc


#### application pour l'apprentissage

[](/classes/dcd/preference.based.pattern.mining.064.png)

- avantage: relativement simple pour les frequents
- on affecte un poids a la transaction en fonction du nombre de motifs dedans
- donc un seul parcours sur la bdd pour le pretraitement
- pour l'echantillonnage, on tire une transaction,
puis on choisit de facon uniforme un motif dedans,
lineaire par rapport au nombre d'items dedans
- pour identifier un tuple une fois qu'il est tire,
on peut le faire par dichotomie → log time
⇒ tirage tres performant dans ce cas la

- par contre, pour des mesures plus sophistiquees,
comme le contraste (2 dernieres lignes),
le pre-traitement sera plus couteux,
voire prohibitif
- pour le contraste, cout quadratique par rapport a la taille de la base
- donc mort a partir d'une certaine taille,
meme si on le fait une seule fois
- donc cette solution n'est pas la seule pour faire
de l'echantillonnage de l'espace de motifs
- mais si le cout est faisable, le tirage est instantane
- pour les approches stochastiques,
on n'a pas de cout de pretraitement,
mais un cout de tirage des motifs
qui depend de la random walk et un peu plus long,
voire on peut ne pas avoir de garanties de convergence vers la solution
- donc encore des problemes ouverts,
mais on a des solutions parfaites pour certaines mesures
