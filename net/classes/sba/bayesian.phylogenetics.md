# bayesian phylogenetics

- analyses de sequences en utilisant des modeles bayesiens,
avec distributions de probabilites et des processus plus compliques
que ce qu'on a vu jusqu'à present


## todo

- redo rec


## intro

- principes de base en phylogenetique moleculaire
- en bas: alignement de sequences traduites en aa
- seq de la chaine alpha de l'hemoglobine, partagee par l'ensemble des eucaryotes
- on arrive a aligner les sequences
- maniere de le voir: ces especes ont evolue a partir d'un ancetre commun
- au cours du temps, mutations -> genes un peu differents
- chaque colonne: position homologue entre les especes
- certaines phylogenies sont plus probables que d'autres
	* avec plus ou moins d'evenements mutationnels
- si un certain groupe, ici vivipares, est un monophylogenetique,
statistiquement ..
- plus facile a expliquer sur une phylogenie comme celle a gauche
que celle a droite
- principe: reconstruire une phylogenie qui minimise
le nombre d'evenements total
qu'il faut invoquer pour expliquer l'alignement


## phylogenetic noise

- evenements: processus stochastiques
- une position peut subir des evenements independants,
voire substitutions multiples
- on a de la convergence, de la reversion
- le principe de parcimonie n'est pas toujours fiable
- il faut donc prendre en compte le fait
qu'on peut avoir de la substitution multiple
en utilisant des methodes probabilistes
- soit par maximum de vraisemblance
- soit par des methodes bayesiennes comme on a fait


## modelisation

- une sequence evolue au cours du temps
- notons qu'on regarde de l'adn et non des aa,
plus simple et c'est la que ca se joue
- 1ere hypothese: chaque site evolue de facon independante
- taux constant a premiere approximation
- 2e hypothese: processus markovien:
sachant qu'on est dans un certain etat,
la probabilite de passer vers un autre etat
est independant du reste de l'histoire evolutive,
cad ce qui c'est passe avant


## transitions, transversions

- probabilites differentes pour les deux
- modifications chimiques de type transition plus probables car plus simples
(purine->purine car un noyau purine etc)
- ..


## markov model

- matrice 4x4 de transition d'un nucleotide a un autre
- taux de substitution de N1 -> N2, en 1e-2/1e6 millions d'annees
- les probabilites dependents de variables: r, γ, κ
- r eleve: toutes les probabilites sont elevees:
..
- transition: κ, variable supplementaire, dans la formule
..
- κ > 1: frequence transition > transversion
- γ proche de 0: mutations vers C ou G peu frequentes et vice-versa,
et inversement pour AT
- γ determine le contenu GC
- a l'etat stationnaire, frequence de G et C = γ/2, ..
- quelle est la probabilite qu'un site qui commence en C
termine en A, peu importe ce qui s'est passe au milieu?
- il y a une infinite de trajectoires mutationnelles vers A
- on veut connaitre la probabilite d'arriver en A depuis C
en un temps t (zB 1e7 annees)
⇒ p(i->j|t), i depart, j arrivee, t temps
⇒ = ∑ ..
..


## likelihood

- imaginons qu'on connait la phylogenie
- on a 3 parametres: r, κ, γ + topologie phylogenique
- D les donnees: alignement observe
..
- quelle est la probabilite que le modele produise les donnees D
sachant les parametres du modele?
- ceci definit la vraisemblance
- si on peut calculer cette probabilite,
on connait la distribution de la vraisemblance
et on peut faire du monte carlo
..
..
- on peut simuler l'alignement site par site
grace a la premiere hypothese d'independance des sites
..
- maintenant comment calculer la probabilite
qu'on observe une colonne en particulier ..


### note: sequences ancestrales

- on ne part pas d'une sequence ancestrale,
cad on ne suppose pas qu'on l'a
..
..


## site likelihood

..
- on a une infinite de departs et de chemins pour arriver a un resultat
- exponentielle de matrice tout a l'heure:
on sait faire ca par branche
..
- on va le faire de facon recursive


## general recursive formula

- formalisme mathematique de l'idee precedente
..
- L_a^n: vecteur de 4 probabilites d'observer toutes les ..
..
- vraisemblances conditionnelles
- c'est une equation recursive
..
..


## pruning algorithm: 1981, felsenstein

..
- avec quelle probabilite est-on parti de A, C, G ou T?
- on prend l'etat stationnaire et probabilite d'avoir des GC ou AT
..
- au bout du compte, tout ceci permet de calculer ..
pour une seule colonne
- calcul pour chaque colonne puis produit
⇒ probabilite de l'ensemble de l'arbre
→ beaucoup de calculs, on doit traverser
..
- gros calcul qui donne une vraisemblance pour l'arbre et pour les parametres


## bayesian inference

- on a la vraisemblance, mais on a besoin de plus d'un prior
- une fois qu'on a le prior,
on obtient le posterior via le theoreme de bayes,
et on peut faire du monte carlo dessus,
pour echantillonner des valeurs de parametres dans la posterior
- zB pour taux de GC on pourrait prendre une distribution uniforme
- de facon generale on peut mettre des prior peu ou pas informatives
en phylogenie,
marche assez bien meme avec peu d'informations,
car on a assez de donnees genetiques
..
- mais comment definit-on un prior sur un arbre phylogenetique,
cad un prior sur tous les arbres phylogenetiques possibles?
..
- quelle prior pour les phylogenies?
..
- on va proposer une nouvelle perspective plus developee
que de prendre des prior uniformes pour tout


## priors on tree

- arbre resultant ..
- tendance a faire une speciation/extension selon les taux λ/μ
- λ > μ: groupe en expansion demographique
- λ < μ: groupe aura la tendance de s'eteindre
..
- a chaque fois qu'une ..
vont a leur tour faire des speciations/extensions
- plein de lignages s'eteignent, d'autres survivent
- phylogenie reconstruite uniquement a partir des especes modernes
- phylogenie T produite par un processus stochastique
a deux parametres λ, μ
- on peut calculer via des equations differentielles ..
- ceci nous donne une prior
- processus qui nous donne une distribution de probabilite
sur tous les arbres possibles,
donc on peut avoir la probabilite de produire un arbre en particulier
..


## structure of complete model: DAG

- on a deja des modeles assez complexes
- mais pas different de ce qu'on a fait dans les cours d'avant
..
- λ et μ peuvent avoir des prior vaguement informatifs,
par exemple loi cauchy ou exponentielles
pour avoir des taux de 1/1e6 etc,
donc assez large ..
- pareil pour les 3 parametres de vitesse d'evolution
- ces 3 parametres permettent de calculer la matrice,
qui est une fonction deterministe du modele
⇒ traits en pointilles
- la phylogenie reste stochastique, une variable aleatoire
..
- on peut definir un processus Q
le long de la phylogenie T
..
..
- question: ..
..
..
..


## ..


## monte carlo on tree

..
