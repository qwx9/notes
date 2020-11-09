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
