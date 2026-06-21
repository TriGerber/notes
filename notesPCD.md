# Chapitre 1

1.select data -> preprocess -> transform -> model -> test

1. Place de la PCD dans les
   processus d’analyse et de
   modélisation des données
   1. Problème, question, tâche
   2. Origine des données brutes - données internes (meilleures, peut demander aux collègues d'en collecter) vs externes (scraping, refresh constant, accès, vérification)
   3. 1. Comprendre les données
      2. Nettoyer les données (data cleaning) - 50%-66% du temps de travail du data engineer. Combiner méthodes formelles et heuristiques empiriques, chercher outliers, valeurs manquantes, ... -> données propres et utilisables en ML
      3. Extraire les attributs (feature engineering) - dériver données et attributs (informatifs, facilitent modélisation / généralisation / etc., meilleure interprétation humaine), faciles à extraire des données, dépendant de la tâche envisagée. Exemple : dériver jour, mois, année, anniversaire, etc, à partir d'une date, supprimer / insérer / combiner des colonnes ! Feature engineering = colonnes, data cleaning = lignes !
   4. Modèles ML: ARN, APV, APN, MIN
   5. Tests d'algorithmes de ML - essayer différents, évaluer -> scores de performance, algorithmes qui auto select les attributs intéressants
2. Étapes d'un projet en ML
   1. Poser le problème et étudier le cadre général - objectif, utilisation prévue, autres solutions, similaires et leurs solutions, mesure de la performance, comment résoudre à la main
   2. Obtenir les données - quelles données nécessaires & leur quantité, origine des données, autorisations, conversion dans un format facile à manipuler, infos sensibles, sous-ensemble test. ! automatiser au max ces traitements !
   3. Étudier les données pour trouver des idées de traitement - ! travailler sur une copie des données ! étudier les attributs (bruit, type, manquantes, pertinence), si ML supervisé, attribut cible que le système doit prédire. comment résoudre à la main
   4. Transformer les données pour les rendre plus accessibles au ML -outliers, feature engineering, données utilisables par ML
   5. Explorer et sélectionner les meilleurs modèles ML - performance, erreurs, selon objectif
   6. Optimiser le système de Ml - optimiser attributs dérivés et hyperparamètres, utiliser données de test
   7. Présenter la solution à l'équipe
   8. Prod

# Chapitre 2

1. Nettoyage des données: détecter, diagnostiquer, corriger, supprimer des outliers, erreurs. Se fait AVANT de traiter les données manquantes et AVANT de transformer les attributs des données.

- scripts bas niveau
- analyse exploratoire à la main

2. Méthodes de détection des valeurs atypiques **numériques**
   1. À l'oeil. Trier les données et regarder les extrêmes. Premier aperçu
   2. À l'oeil. Boxplots / histogrammes
   3. Nuages de points -> correlation -> détection d'outliers qui ne sont pas sur la fitted line (droite de régression). Si le but est de faire de la régression, alors ces outliers seront des problèmes.
   4. Score Z = (X - moyenne) / écart-type -> seuil standard |Z| > 3 -> probabilité de 0.027% que ce soit le cas
   5. Étendue interquartile EI = IQR ->
      1. Seuil intérieurs inférieur / supérieurs => Q1 - 1.5 x IQR et Q3 + 1.5 x IQR => valeurs atypiques mineures
      2. Seuils extérieurs inférieurs / supérieurs => Q1 - 3 x IQR et Q3 + 3 x IQR
   6. Test d'hypothèse (test de Grubbs)
      1. H0: il n’y a aucune valeur atypique
      2. Ha: la valeur X qui maximise la statistique G est atypique, avec G = |X – | / 
   7. Algorithme LOF (Local Outlier Factor)
      1. Score LOF = rapport entre moyenne de densités locales autour d'un point et moyenne de densités des k plus proches voisins -> est-ce qu'on est au centre du cluster ou un outlier ?
      2. Densité locale = nb de points dans un rayon fixé autour du point.
   8. Automatiquement désigner des valeurs en dehors d'un intervalle comme atypiques, e.g. > 24h
   9. Autre : Kurtosis (Distribution de valeurs normalisées), Densité (K-NN/LOF)

Méthodes 3 à 7 : multivariées
Les autres : univariées

Pas une méthode meilleure, récente, parfaite. le choix dépend des données atypiques, du résultat attendu, du contexte.

Data quality problems:

1. Single-Source Problems
   1. Schema Level (manque d'intégrité de contraintes, mauvais design)
   2. Instance Level (Erreur d'entrées dans les données) : typo, données dupliquées ou contradictoires.
2. Multi-Source Problems
   1. Schema Level (modèles et designs hétérogènes) : conflits de noms ou structure.
   2. Instace Level (Overlaps, contradictions et inconsistances) : aggrégats et timing inconsistants.

Point 4. Processus de nettoyage :identification, diagnostic, correction
screening -> diagnosis -> editing

1. Identification
   1. Données manquantes, double
   2. aberrantes incohérentes -- données fausses mais pas aberrantes
   3. distributions étranges
   4. résultats surprenants ML
   5. en général, être à l'affut de ce qui diffère des attentes
   6. Données structurées
      1. Présence obligatoire (NULLABLE?)
      2. Contraintes sur le type (Bool, Date, etc.)
      3. Unicité valeurs
      4. Appartient à ensemble
      5. Contraintes de validité
      6. Détection de duplica

2. Diagnostic - Expliquer la cause des données atypiques / distributions anormales
3. Correction - Que faire avec les données erronées -> corriger / supprimer / conserver (remplacer par la moyenne, outlier cohérent on garde, deuxième mesure, moyenne avec résultat proche, supprimer une année de covid car perturbe les modèles)
   1. Bien documenter, comprendre la cause avant de supprimer.
   2. Données atypiques peuvent être réelles, p.ex critical failure qui n'arrive qu'une fois.

# Chapitre 3 - Traitement des données manquantes

Généralités

## Causes de données manquantes :

- Humains n'ont pas répondu
- Données critiques qui ne peuvent pas être publiées
- Info pas disponible
- Collecte des données mal faite
- Problèmes techniques sur les équipements de récolte

1. Manquant totalement aléatoirement : indépendant de tout paramètre observable -> ignorer et faire les stats sur les données actuelles
2. Manquant aléatoirement : dépend de certains attributs, ex: hommes répondent moins que femmes -> analyser, modéliser, utiliser.
3. Manquant non aléatoirement : cause dépend de l'attribut de la valeur, ex: batterie fail quand trop froid -> difficile à corriger

D'abord on regarde les correlations, on essaie de comprendre pourquoi elles manquent. Si on trouve rien, alors aléatoire.

### Que faire en cas de données manquantes ?

éviter absences dès le départ -> effacer tout le pt de donnée / générer val aléatoire=imputation -> gérer absence avec méthodes d'analyse: max-margin, max-vraisemblance, expectation-max

Avec numpy, valeurs manquantes = np.nan
np.isnan (true/false, [true/false, true/false, ...]), nansum (nan=0), nanprod (nan=1)

## Suppression des valeurs manquantes

1. suppression complète :
   - facile à implémenter
   - biais, sauf dans le cas MCAR (missing completely at random)
   - réduit la taille des données -> décroît la confiance dans les stats
2. suppression sélective : supprimer seulement pour les analyses qui utilisent ceux qui manquent, mais les garder plur les autres analyses
   - permet de garder plus de données, mais taille variera selon analyses
3. suppression de colonnes : si plus de X% des valeurs sont manquantes

Code :

1. Méthode .dropna() :
2. data = pd.Series[1, np.nan, 3.5, ...]
   data = data[data.notna()]

## Méthodes d'imputation univariées

univariée = on regarde que la colonne concernée pour estimer les valeurs

1. donner une étiquette spécifique aux valeurs manquantes, p.ex "missing"
2. tirer au sort une valeur parmi les présentes
3. réutiliser la dernière valeur (triées)
4. moyenne des valeurs de la colonne, ou des n dernières et n prochaines, à voir selon dataset

Code :

1. .fillna() - fill les n/a avec une valeur donnée
2. .ffill(), .bfill() - vers l'avant avec dernière val f / vers l'arrière b
3. .interpolate() - imputer par interpolation
4. scikit-learn : impute.SimpleImputer() -> imp.fit(...) : entrainement, calcule moy/col, imp.transform(array) : remplace nan par moy

## Méthodes d'imputation multivariées

multivariée = on regarde plusieurs colonnes pour estimer les valeurs

1. Régression (fit) basée sur les autres attributs des données (autres colonnes) et utiliser le modèle pour prédire (fit_transform). ! relations plus fortes qu'en réalité -> régression stochastique
2. Non-negative matrix factorization
3. Moyenne de la valeur pour les knn.
   from sklearn.impute import KNNImputer : KNNImputer(n_neighbors=2, weights="uniform") -> imp.fit() / imp.fit_transform(). Connaitre la pos des nan -> MissingIndicator
4. SimpleImputer()
   - median
   - mean
   - ...
5. IterativeImputer(max_iter=..., random_state=...) -> imp.fit(train) -> imp.transform(test)
   - BayesianRidge
   - DecisionTreeRegressor
   - ExtraTreesRegressor
   - KNeighboursRegressor
     Dépend du jeu de données et de l'objectif + tests

# Chapitre 4 - Transformation des données

- Dataframes (items, observations), instance, attributs
- chercher attributs qui répondent à notre question
- Transformation des attributs
  - manuelle: combiner, diviser,transformer attr -> nouveaux attr, plus performants
  - automatique: sélection d'attributs, NN, nouveaux attributs

- Textes -> sac de mots, coeff TF-IDF
- Audio -> Fourier, MFCC
- Images -> lignes, frontières, angles, textures, HIFT, HOG
- Séries temporelles -> attributs calculés, moyenne, iqr, rms, ...

Categorical de Pandas : p.ex transformer Yeux | 'bleus', 'verts', 'bleus' | -> Yeux | 0, 1, 0 |
qualif = pd.Categorical([‘bien’, ‘très bien’, ‘bien’, ‘passable’, ‘bien’])
df['col'].astype('category', ordered=False) --> df['col'].dtype

--> variables dummies (factices -> ajouter des colonnes à partir de colonnes existantes) / one-hot (eyes*blue)
pd.get_dummies(data, prefix='yeux', prefix_sep='*', dummy_na=False, columns=None, drop_first=False)

2. Intervalles -- bins, baquets
   1. nombre fixe -> n bins, diviser l'intervalle [Xmin - E, Xmax + E], E étant une marge pour les extrêmes (pourcentage de la taille de l'intervalle).
      pd.cut(x, bins, right=True, labels=None, retbins=False, precision=3, include_lowest=False)

   2. Intervalles custom -> tailles différentes, selon valeurs, etc. P.ex : tranches d'âge
   3. séparation par quantiles -> q = [0, .25, .5, .75, 1]
      pandas.qcut(X, q, labels=None, retbins=False, precision=3, duplicates='raise')

Utiliser log pour X pour le binning si X est dispersé vers de très grandes valeurs -> uniformise la distribution
Box-Cox avec scipy.stats -> utiliser df.apply(lambda x: ((x-x.mean())/x.var()))
== en sklearn :

3. Transformer données structurées

- Jointures
- GroupBy

4. Gestion des duplications et échantillonnage
   DataFrame.duplicated(subset = None, keep = 'first') :
   - return bool pour chaque ligne dupl
   - keep = 'first' -> garde la première ligne
   - colonne ou liste de noms à utiliser pour le test

# Chapitre 5 - Analyse

1. Analyse en facteurs : combiner des attributs pour obtenir des facteurs.
   - En priorité avec grande variance car on peut bien séparer les données, et signaux forts
   - Éviter les attributs avec correlation car redondants

2. Définition - comment on sait quoi prendre, comment on prend

ACP : Analyse en Composantes Principales
- 1ère CP: droite de variance maximale, vecteur dans la direction où les données varient le plus.
- 2ème CP: direction suivante orthogonale à la CP1 de plus grande variabilité une fois la CP1 enlevée
- etc. jusqu'à ce qu'on ait la variance souhaitée
--> vecteur unitaire u qui maximise la variance -> gros calcul matriciel
--> vecteur propre

On essaie d'avoir un petit nombre de CP qui représentent p.ex 80% de la variance.


# Chapitre 6 - Sélection des attributs
Sélectionner un sous ensemble optimal d'attributs -> réduire le nombre de dimensions.

Méthodes:
1. filtrage

sklearn: SelectKBest, SelectPercentile

P.ex calculer le coeff de correlation avec les différents inputs, puis sélectionner ceux qui ont les plus hautes valeurs.

2. wrappers (modulaire)

   SequentialFeatureSelector: direction('forward' ou 'backward'), n_features_to_select, méthode 'Transform'

   teste un module de ML: 
   NO : recherche exhaustive (examiner tous les sous ensembles)
   YES : recherche séquentielle
   -->
   Recursive Feature Elimination: recherche décrémentale qui fournit des scores d'importance après entrainement.
   RFE: entraine le ml, supprime l'attribut le moins important, recommence n fois avec d'autres features
   RFECV: tous les nb possibles d'attributs, puis choisit le meilleur en termes de scores du module de ML, avec validation croisée.



3. intégrées aux algos de ML
recherche séquentielle incrémentale : 1 à m -> prendre les m attributs qui ont le score le plus élevé en entrainant un modèle beaucoup trop de fois
' ' décrémentale : supprimer les attributs au lieu de les ajouter.
- LASSO: régression linéaire + pénalité si trop d'attributs
- Recursuve Feature Elimination
implémentation: pipeline -> feature_selection: SelectFromModel(...), puis classification: RandomForestClass...

# Chapitre 7

7. Images
Systèmes de coordonnées
   - raster : commencer en haut à gauche, balayage de gauche à droite haut en bas
   - cartésiennes : en bas à gauche à l'origine, x, y
   - cartésiennes avec origine au centre.
MNIST: chiffres de 0 à 9; CIFAR10: 6 animaux, 4 véhicules; CIFAR100: 100 classes avec 20 classes * 5 sous-classes

- Classer sans attributs; juste avec des pixels -> kNN (nul)
- Histogramme des intensités en gris ou r / g / b : P(I) = N(I) / N -> probabilité d'avoir une certaine intensité -> intensité moyenne, disperstion, entropie(dispersion, -Sum(P(I)log(P(I)))), variance (ordre 2), skewness (ordre 3), kurtosis (ordre 4)
- Segmentation en composantes connexes : identifier des blobs, des objets qui ont des pixels connectés
- Filtrage : matrice 3x3 -> 1 valeur=somme des 9.
   - Prewitt : détecte valeurs de la matrice horizontal / vertical pour détecter des lignes verticales/horizontales
   - Sobel : approx. gaussienne : dérivée du changement d'intensité (h/v) puis diviser par 8 -> matrice 3x3
- Lissage : moyenne des pixels adjacents
   - Filtre gaussien : poids des voisins avec un poids décroissant selon la distance des voisins.
- Pyramide gaussienne : ajouté à l'image originale un floutage puis réduction, e.g. 1/2, 1/4, 1/8, 1/16, ...
- Matrices de gradients (directions d'intensité) : SIFT : patterns de gradients
   - SIFT: 16x16 pixels -> gradients de chaque pixel -> 16x4x4 -> 16 sous-fenêtres x 8 orientations -> angle histogram -> donne des vecteurs pour classification contenant les angles.
   -> appariement des descripteurs entre deux images
- Réseaux convolutifs - feature learning

8. Textes

9. Audio

10. Séries temporelles

11. Systèmes de recommandation



# Chapitre 8 - Attributs pour les données textuelles

- Tokenisation
- Lemming : enlever les temps, tout mettre au masculin, au singulier
- Stemming : réduire aux racines
Mots inconnus ou fautes d'orthographes ett 
--> rajouter des subwords. Dans le réseau, on a p.ex 20k mots, 10k subwords, 200 lettres/ponctuation/chiffres
on construit les subwords avec p.ex les n fragments les plus fréquents dans tout le dataset.
'E' -> 'EC' (plus fréquent)
'EC' -> 'ECA' (plus fréquent) -> ... -> 'ECART'

- Vectorizers:
   - Count Vectorizer : tableau avec colonnes = documents, lignes = mots, et dans les cases le count du mot/subword/autre dans les documents. --> matrice termes-documents

vecteurs sentence-BERT : vecteurs qui permettent de détecter la similarité entre des phrases qui ont un sens similaire avec du ML. (sentence_transformers -> model = SentenceTransformer("all-MiniLM-L6-v2"), puis model.encode(sentences), puis model.similarity)

# Chapitre 9 - Audio

Attributs temporels : 
- enveloppe du signal : valeur absolue maximale du signal
- RMS (root mean square) : moyenne des carrés des décibels mesurés
- ZCR (zero-crossing rate) : nombre de fois qu'un signal passe par 0 (plus faible sur les voyelles / instruments, plus grand  certaines consonnes, percussions, ...)
- fourier transform STFC
- MFCC : coefficients, spectre du son sur court terme
- Mel-frequency kepstre Cepstrum Log power transform, MFCC.
- STFT : intensité de chaque fréquence dans chaque fenêtre = carré du module du coeff fourier

S Composante saisonnière
p.ex dimanche chute de ventes
Variations saisonnières : Additives, Multiplicatives. Causes : saisons, fêtes, etc.
Effet calendaires : weekend, vacances, fêtes qui changent de jours chaque année et weekends qui se décalent chaque année -> si on cherche par mois, problème.

T tendance
sur le long terme, ça augmente / diminue, évolution long terme

C Composante cyclique
fluctuations à long terme qui se répètent, mais pas fixe
p.ex augmentation de la bourse puis crash, des fois en un jour, des fois en un mois

I irrégulière.
quand on a tout enlevé, c'est un bruit, p.ex le hasard

Modèle additif d'une série Y : Y_t = S_t + T_t + C_t + I_t

1. détecter puis enlever la c. saisionnière, p.ex avec un lissage
- Lissage:
   - moyenne mobile : moyenne sur n précédents et n suivants
   - exponentiel : impact décroissant exponentiellement des prédécesseurs E1 = Y1 -> E2 = wY_t + (1-w)E_t
      exponential prévision  : F_t+1 = F_t + w(Y_t - F_t)
   - exponentiel de Holt : permet de saisir la tendance d'une série et variation saisonnière avec 2 eq et 2 const. 
2. détecter puis enlever la tendance, p.ex avec une régression
3. détecter puis enlever la c. cyclique, p.ex en considérant le rapport Y_t/T_t
4. Il reste la c. irrégulière (résidu ou bruit)

