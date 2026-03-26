sommet = vertex
sommets = vertices

Graphe non orienté:

- ensemble V (sommets ou noeuds)
- Ensemble E (arêtes)
- fonction d'incidence associe à arrête e une paire {u(e), v(e)}

Graphe non orienté:

- ensemble V (sommets ou noeuds)
- Ensemble E (arcs) -> direction
- fonction d'incidence associe à arrête e un couple (u(e), v(e)) => Extrémités de l'arc e.

multigraphe vs pseudographe, multigraphe non-orienté
graphe simple : pas de boucle, pas de parallèle.

graphe sous-jacent = transformer arcs en arêtes.

couple vs paire ?
2 extrémités identiques = boucle
liens parallèles entre deux sommets (passer par lausanne vs payerne) !!! même extrémité finale et initiale -> liens multiples
arcs de sens opposés = aller retour -> liens non multiples -> simples

Plus petit -> plus grand:
graphe nul : ensemble nul
graphe vide : n sommets, pas d'arête, pas d'arc
graphe trivial : 1 sommet, pas d'arête, pas d'arc
graphe fini : nombre fini de sommets / arcs / arêtes
graphe infini : nombre infini de sommets / arc / arêtes

degré d'un sommet v - deg(v) : nb arcs ou arêtes incidents au sommet v. !! boucle compte double.

arcs:
demi-degré sortant : nb d'arc sortant du sommet
demi-degré entrant : '' entrant ''

somme des degrés = double du nombre d'arêtes / arcs de G. Logique car chaque arc / arête a 2 extrémités qui se compte dans les degrés.
double -> pair

graphe partiel : G' = (V,E') graphe partiel de G = (V,E) <=> E' C\_ E. On conserve tous les sommets de G et un sous-ensemble de ses arêtes ou de ses arcs. -> copie du graphe avec des liens en moins ou pareil.

sous-graphe : sous-ensemble W de sommets appartenant G avec toutes ses liens qui sont liés à ces sommets

sous-graphe partiel : sous-ensemble du graphe G avec moins de sommets et moins de liens.

chaîne : suite alternée de sommets et **d'arêtes** commence et termine par des sommets, chaque arête encadrée par deux extrémités.
cycle : **chaîne** fermée qui se commence et termine depuis le même sommet (deux extrémités confondues) et compte au moins une arête.

chemin : suite de sommets et **d'arcs** où chaque arc est précédé par son extrémité initiale et suivi par son extrémité finale (il y a un sens)
circuit : chemin fermé dont les deux extrémités sont confondues (extrémités initiale et finale du chemin)

longueur : nombre d'arêtes / arcs
élémentaire : chaque sommet au plus 1 fois
simple : chaque arête / arc au plus 1 fois

Théorème Erdös-Gallai pour savoir si une suite de degrés est issue d'un graphe simple

Graphe régulier degré r si tous les sommets sont de degré r.

forêt (non-orienté, pas de père,fils,etc.) = graphe sans cycle simple (normalement cycle peut être simple aller retour avec la même arête)

matrice adjacence : matrice représentant la relation des sommets entre eux.
Matrice d'incidence : matrice sans boucle représentant la relation entre les sommets et les arêtes. Arcs -> sans boucle, +1 / -1 selon direction.

Graphe peu dense : nb arêtes / arcs proportionnel à n
Graphe dense : nb arêtes / arcs proportionnel à n^2

- compact (n+m)
- trouver 2 sommets adjacents -> partourir une liste
  Graphe statique (constant) ->

graphe statique -> tableau compact d'adjacence ou de successeurs
Tableaux successeurs :

1. Tableau des indices des successeurs : tableau qui indique le nombre de successeurs que chaque sommet a dans le tableau des successeurs.
2. tableau des successeurs : tableau contenant des sommets étant les successeurs d'autres sommets.

Exemple : TIPS = {0, 2, 4, 4, 6, 7} -> v1 a {0, 1}, v2 a {2, 3}, v3 a 4, v4 a {4, 5} (donc v3 a une taille nulle), etc.

graphe planaire = représentation d'un graphe dans le plan avec aucune arête ou arc qui se coupent -> peut être manipulé plus efficacement par certains algos

Théorème 2.1
Graphe non-orienté G et sa matrice d'adjacence A. Le nb de chaînes (sommet vi à sommet vj) de longueur k entre vi et vj est égal à l'élément (i,j) de A^k.

Pour k=0, A^k = A^0 = I -> 0 chaîne de longueur 0 entre vi =! vj et 1 si vi = vj.
Exemple, chaîne entre v1 et v4 de longueur 4 -> A^4

Pas d'induction (vérifier que si c'est vrai en A^k, k>=0 alors c'est aussi vrai en A^(k+1))
(A^(k+1))ij = Sum[l=1, n](A^k)il\*(A)lj = Somme, pour tous les sommets l, du nb de chaînes de longueur k+1 entre les sommets vi et vj et avec vl comme avant-dernier sommet

Comportement asymptotique d'une fonction :
Ordre supérieur vs inférieur : si à partir d'un certain point constant, f est toujours plus grande ou égale à g, alors f est d'ordre supérieur à g. Pas forcément besoin que f soit supérieure ou égale à g en dessous de cette constante. Exemple : racine cubique de x est d'ordre supérieur ou égal à la fonction ln(x).
xln(x) est entre x et x^2, donc il est d'ordre supérieur à x et inférieur à x^2.

Landau :
f est d'ordre inférieur ou égal à g -> f £ O(g) ou f = O(g)
f est d'ordre supérieur ou égal à g -> f £ Omega(g) ou f = Omega(g)
f est de même ordre que g -> f £ Phi(g) ou f = Phi(g)

graphe connexe = en un seul morceau

graphe fortement connexe = pour chaque sommet, il y a un chemin revenant à ce sommet en passant par tous les autres sommets. Il a donc une seule composante fortement connexe.

composante fortement connexe = sous-graphe fortement connexe d'un graphe
!! un sommet isolé est fortement connexe, mais il est seul dans son sous-graphe -> C = {v9} !!

graphe réduit = tous les sous-graphes fortement connexes du graphe deviennent des sommets. Exemple : {v1v2} --> {v4v5v6}, {v3} --> {v6v7v10} --> etc.

BFS (Breadth First Search): exploration par pas, progressif, loupe rien, dijkstra like

- tous sommets sont en "non-visités", en blanc
- traiter 1er sommet degré 1
- mettre le 1er sommet degré 1 dans liste de sommets déjà vus
- traiter tous sommets degré 1 et les mettre dans la liste.
- traiter et mettre le premier sommet degré 2 du 1er sommet degré 1 dans liste de sommets déjà vus
- traiter tous sommets degré 2 du 1er sommet degré 1
- mettre le sommet degré 1 dans liste de sommets complètement visités
- repeat until the end
- output: tableau de chemin le plus court depuis le sommet initial pour chaque sommet, et tableau indiquant par quel sommet on est passé juste avant d'y arriver

DFS (Depth First Search): on s'enfonce le plus possible, récursif.

- sommet initial --> premier sommet lié --> premier sommet lié --> récursif.
- si plus de sommets à traiter, retour en arrière et traiter les autres
- output: un tableau de début (première fois que le sommet est visité) et un tableau de fin (dernière fois que le sommet est visité)

graphe transposé / inverse : changer le sens de chaque arc

Gabow: 1 parcours en profondeur + 2 piles pour stocker les sommets non encore classifiés

Kosaraju: 2 parcours: 1er en profondeur sur le graphe transposé, puis 2e en largeur sur chacun des sommets en ordre décroissant selon leur indice de fin.

Tarjan: 1 parcours en profondeur + 1 pile + 1 tableau indexé par sommets
1 parcours en profondeur avec un tableau supplémentaire stockant le plus vieil ancêtre. Si on arrive au plus vieil ancêtre et qu'il n'a pas d'ancêtre, alors lui et tous ceux qui l'ont comme plus vieil ancêtre sont dans une composante fortement connexe.
ndfs, low, scc et pile

4. Arbres
   arbre
   == sans cycles et connexe
   == sans cycles et n-1 arêtes
   == connexe et n-1 arêtes
   == chaque paire de sommets de G reliée par une seule chaîne
   == connexe et minimal pour: aucun graphe partiel strict n'est connexe
   == sans cycle et maximal pour:

arbre recouvrant : graphe partiel d'un arbre. Si graphe original est connexe, alors graphe partiel recouvre tout le graphe. Si graphe original non connexe, alors plusieurs sous-graphes -> plusieurs arbres -> forêt recouvrante.

graphe est connexe uniquement s'il possède un arbre recouvrant.

Réseau = graphe + infos supplémentaires
graphe pondéré = poids (particulièrement non-orienté)

recherche d'arbre recouvrant de poids minimum (algorithmes voraces: étape par étape, fait le meilleur choix local sans jamais revenir en arrière, part du plus faible coût et va de plus en plus gros) :

- Algorithme de Kruskal :
  On parcours les arêtes de la moins chère à la plus chère et on essaie de les ajouter à mon arbre. Si ça crée un cycle -> interdit, abort. Si ça ne crée pas un cycle, alors on l'ajoute.
  Complexité : n-1 <= m <=n(n-1)/2 et mEO(n^2) et nEO(m). Première phase de l'algo a un tri -> O(mlogm) O(mlogn) pour graphe simple. Deuxième phase O(mn), donc Kruskal : O(mn)
  Avec Structure Union-Find : O(mlogn + n^2)
  Si on fait des trucs, À DETERMINER !!!!!, alors O(mlogn)

- Algorithme de Prim :
  parcours en largeur, not FIFO mais queue de priorité (coût d'ajout dans l'arbre).
  Quand on ajoute un sommet, on a update une structure (l'arbre) pour qu'il comprenne tous les sommets déjà visités. Ensuite, on regarde le nouveau sommet pour voir s'il existe des sommets qui peuvent s'y connecter depuis l'extérieur de la structure.
  Complexité : queue de priorité ->insert, findmin,extractmin,decreasekey -> Tableau: O(n^2), Tas binaire: O(mlogn), Tas de Fibonacci: O(m + nlogn)

- Algorithme de Boruvka :
  Chaque sommet regarde autour de lui et va vers son voisin le moins cher. Peut être exécuté en parallèle. Cela donne des morceaux. On les agglutine en un seul sommet et on repeat jusqu'à-ce qu'on ait un seul chemin.

Démontrer que Kruskal / Prim sont des algorithmes optimaux :
Coupe : les arêtes/arc allant de A vers !A où on peut couper les sommets liés en deux ensembles avec une droite. Ex:

- T : arbre recouvrant de poids min,
- e : arête de plus petit poids de la coupe (X,!X)
- C : cycle qui se crée en ajoutant e à T
- f : arête de TnC

si on rajoute une arête entre 2 sommets du même sous-ensemble, alors ça crée un cycle.

Structure Union-Find

section inférieure d'une chaîne : arête de plus petit poids de la chaîne. section supérieure : arête de plus grand poids de la chaîne.
-> si on veut débit le plus haut, alors on doit maximiser la section inférieure -> arbre de poids max.
-> si on veut avoir une batterie la plus petite possible, alors on doit avoir des tronçons le plus court possible entre deux stations de recharge -> minimiser la section supérieure -> arbre de poids min.

arborescence : 1 racine et tout est orienté vers l'extérieur. Chaque sommet a exactement 1 prédécesseur sauf la racine qui n'en a aucun.
anti-arborescence : 1 racine et tout est orienté vers l'intérieur. Chaque sommet a exactement 1 successeur sauf la racine qui n'en a aucun.

arborescence / anti-arborescence recouvrante : arbre recouvrant de G qui est une (anti-)arborescence.

Les algos de poids min sont pas min pour tous les sommets recouvrants.
Prim : pour construire argre recouvrant de poids min. On peut l'appliquer dans un graphe orienté en choisissant un sommet et connectant les successeurs. Version orientée (Kruska peut pas).
Poids = 20

Djikstra : arborescence de CHEMIN de longueur minimale.
Poids = 21

Chu-Liu : Poids = 15 --> Vrai poids minimum

- Arcs entrants de plus petit poids
- Regrouper par cycles et nouveaux arcs = arc - arc entrant du sommet
- Repeat jusqu'à-ce qu'on ait que 2 cycles et 1 lien.
- Regonfler

Triangulation de Delaunay :
Si on a 4 points -> 2 trianges de 3 points, alors on a 2 cercles de 3 sommets.
Si les cercles ont le même nombre de sommets à l'intérieur que de sommets sur leurs arêtes, alors ils appartiennent à la triangulation de Delaunay.
"Sur un cycle, l'arête la plus lourde n'est pas dans un arbre recouvrant de poids min"
Avec cette méthode, on voit direct si le cycle est de poids min, si pour toutes les arêtes de l'arbre recouvrant appartiennent à la triangulation de Delaunay.

plus court chemin quand on a une pondération -> la longueur d'un chemin ou circuit correspond à la somme des poids de ses arcs

Bellman-Ford :
Lj est la distance entre s et j = longueur d'un plus court chemin de s à j.
-> Lj <= Li + Cij pour tout i appartenant aux prédécesseurs de j.
Lj'k = longueur de plus court chemin de s à j utilisant au plus k arcs.
Lj'0 = infini sauf pour j=s où c'est 0.
on calcule pour chaque sommet j les valeurs de Lj'1, Lj'2, etc.
On copie les résultats de l'itération précédente dans une nouvelle itération, etc.

On peut utiliser un seul lambda et écraser l'indice à chaque itération : si Lj > Li + Cij, alors Lj = Li + Cij.

pour coder :

- parent[j] = predecesseur de j
- Condition d'arêt : si aucune diminution des marques Lj n'a lieu pendant une itération complète, ou après n itérations

Djikstra : Prim mais on garde les poids cumulés.

- Ne supporte pas les poids négatifs
- On connait le nombre d'itérations

Avec poids constants, simple parcours en largeur.

Bellman-Ford : Plus court chemin, Label **correcting** algorithm -> on sait pas si c'est optimal quand on trouve une solution
Dijkstra : Plus court chemin, Label **fixating** -> permet de s'arrêter quand on trouve la solution

Circuit absorbant = circuit à poids négatifs
Pour retracer un circuit absorbant, il faut p.ex, ajouter un compteur par sommet qui s'incrémente chaque fois qu'il est visité.

Bellman :
1. chaque diminution de lambda, stocker le sommet dont la marque vient de diminuer (ou son père)
2. Sans stabilisation des marques après n itérations, empiler les sommets en partant de start en remontant les marques p jusqu'à retomber sur un sommet déjà dans la pile.
3. Construire le circuit absorbant en remontant la pile jusqu'à-ce qu'elle soit vide

un sommet qui a vu sa marque lambda diminuer durant l'itération n.

Variations de Bellman-Ford utilisées à chaque fois :
Sans circuits absorbants : générique
Avec circuits absorbants : version modifiée 5.18

Si on a des poids de transit, sur des sommets, non orienté, alors 3 possibilités :
1. Passer en mode orienté en ajoutant deux arcs au lieu de l'arête et mettre les poids sortant ou entrant sur les arcs
2. orienter graphe en ajoutant deux arcs opposés au lieu de l'arête, poids nul, remplacer sommets par entrée u', ... ?
3. Rester en mode non-orienté et faire la moyenne / additionner les poids des deux sommets d'extrémité.

log base entre 0 et 1 -> x->0 = +infini, x->inf = -infini, fonction strictement décroissante
log base > 1 -> x->0 = -infini, x->inf = +infini, fonction strictement croissante

