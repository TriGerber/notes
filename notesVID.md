## robuste / non-robuste

- robuste: étendue interquartile (rangs, puis positions (lire la valeur))
- non-robuste: étendue, écart-type (au carré par rapport à la moyenne) -> moyenne non robuste, variance (carré de l'écart-type)

## table des fréquences = table de contingence

utilisé dans les manufactures -> une étendue, plus petite possible, maximum - minimum. !! pour les choses qui sont strictes, où il doit y avoir un max et min, on peut pas utiliser quartiles car c'est Q1 et Q3.

Diagonale doit être grande, et valeurs hors de la diagonale moins grandes.

## Distributions symétriques

Curtosis : Distance avec la distribution normale. Distribution normale = Kurtosis 0. Logistic distribution = Kurtosis 1.2, Uniform distribution = Kurtosis -1.2, raised cosine districution = Kurtosis -0.59, ...

Plus c'est haut et grand, plus la queue est lourde.

## coeff de corrélation linéaire r

mesure de la linéarité. r=-1 relation linéaire inverse (correlation négative), r = 1 -> relation linéaire directe (correlation positive)

## coeff de corrélation de Pearson

linéaire, soit il existe une relation, soit il n'en existe pas. -> pas quadratique, pas robuste

## coeff de Spearman / coeff de Kendall

- coeff de Spearman : robuste,
- coeff de Kendall : robuste, rangs

## Tufte / Data storytelling / Rosling

- Tufte : soft, dit: beaucoup trop chargé, pas assez soft, trop de blanc
- Data storytelling : chargé
- Rosling: cartes, bulles

## Diagramme ne violon

bon pour voir la distribution (reproche qu'on fait au diagramme boite à moustache = rigide)


# Régression Linéaire

Paramètres optimaux :
β_0 : ordonnée à l'origine (biais qui décale la droite)
β_1 : pente, coeff de xi.

^β_0 et ^β_1 => paramètres estimés. = output de coef(object)
^y_i = valeurs ajustées (estimées)

r_i = y_i − ^y_i = résidus = différences entre réel et prédit

Le but de la régression linéaire simple est de réduire l'écart entre chaque y_i et leur valeur estimée.
Somme des carrés des résidus, à minimiser pour les moindres carrés : Somme_i((y_i - (β_0 + β_1 · x_i))^2) == deviance(object)
== Somme_i((y_i-^y_i)^2)


==> estimation avec la méthode des moindes carrés
==> validation avec l'analyse des résidus

coeff de détermination (validation du modèle) avec R^2 = coeff de corrélation entre x et y :
R^2_adj = 1 − (1 − R^2) * ( n − 1 / n − 2)
-> donne la variabilité des prédictions (imparfait, mais utile)

Vérification avec des graphiques :
1 la linéarité de la relation
2 la nullité de l’espérance des erreurs ε_i et leur variance constante σ^2;
3 la normalité des variables aléatoires erreurs ε_i.

T = ^B_1 / ^sigma(^b_1) = valeur prédite divisé par écart-type de valeur prédite

P-value = 2 * P( T > | t_observe | H_0 ):

10% / 5% exploratoire
1% pour confirmation

région de non-rejet : H0: B1=0
région de rejet : si t_obs est dans : t_n-2, 1-(a/2)


Hypothèse H0 et H1 = deux cas possibles, 1 ou 0, vrai ou faux.

Si on est en H0 (moins de 5% sur P-value), alors la variable explicative n'est pas assez importante.
Si on est au dessus de 5%, on est en H1, et 

The smaller the p-vaule, the stronger the evidence against H_0

H1: B1 > 0 --> 1 - 0.025 = 0.975
--> regarder dans la table de student
v = degré de liberté = nombre de données observées

p-valeur = 0.087
degré de certitude = 0.05
0.087 > 0.05 --> on ne rejette pas, H0

p-value faible -> H1
p-value forte -> H0




business understanding : understand the data & the problem
data understanding : aberrant values, 

typical presentation:

contexte
crisp
cohérence des données, données atypiques / manquantes, etc.
what was done
key results
confusion matrix
diagrammes en barre
recommendations pour le client
conclusion

report:
intro
business understanding (business obj ,quest.)
... crisp
code in appendice

EmilHvitfeld extensions pour présentations