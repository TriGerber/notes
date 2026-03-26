## Voir ce qui contribue le plus à la bonne prédiction -> améliorer

### 1. Exploratory Data Analysis

regarder les données, déterminer si les outliers sont des erreurs ou peuvent être enlevés, car outliers perturbe l'entrainement

```text
Trouver outliers -> analyser outliers -> traiter
trouver N/A -> analyser N/A -> traiter
```

### 2. pre-pocessing

```text
x' = (x-min) / max-min
```

## Feature engineering

transformer des données,

Exemples:

- Surface & prix -> Prix/Surface
- Son -> FFT (Fourier Transform)
- Texte -> keyword frequency
- Image -> color histogram
- heart beat (time-series) -> heart frequency

## Supervised learning

```text
data collection -> pre-processing -> feature extraction -> model selection -> perf evaluation
```

overfit (trop d'entrainement sur un même dataset, learning by heart) / underfit (trop général / sous entrainé)

faire une grille avec tous les paramètres, prendre quelques points au hasard et rechercher le point le plus optimisé

## Mesures perf

- Accuracy = TP+TN / TP+TN+FP+FN (tout)
- Precision = TP / TP+FP
- Recall = TP / TP+FN
- F-score
- confusion matrix

## Perceptron

Neuron: If (x1+x2+x3+...) >= n, then y=1, else y=0. n est le seuil d'activation. x sont les inputs du neurone.

Weights: If (w1x1+w2x2+w3x3+...) >= n, then y=1, else y=-1

Bias: poids extra pour décaler la droite séparant les classes. Sans bias, la droite passe toujours par l'origine.

```text
wt*x + w0 = (0.6;-0.2)horizontal*(2;1)vertical => 1.2 - 0.2 - 0.1
```

!! Revoir matrices et opéraitons sur matrices !!

1. randomly initialize weights
2. compute y (output) = sum(wixi) (input)
3. update weights: Wi(t+1) = Wi(t) + u(d-y)x avec d = desired output et u = learning rate
4. repeat until error < treshold

```text
E = 1/2(d-y)^2 -> Ecart
```

### Exemple

```text
y = wtx (0.2; -0.5)(1, 2) = 0,2 -1 = -0.8
y-d = -0.8 - 1 = -1.8 <==> d-y = 1.8
w2 = (0.2;-0.5) + 0.1(1.8)(1;2) = (0.38;-0.14)
E = 1.62
```

## Gradient descent

new model; ADALINE (adaptive linear element)

```text
y = sum(wixi)
E = error function = 1/2sum(i=1, p){(d-y)^2}, ...
```

seuil d'activation -> problème pour la continuité -> peut pas faire de gradient descent.

biais = décale le point x=0 sur l'axe y.

## Widrow-Hoff algorithm / Delta rule

Gradient descent

- plus un 1 ou 0 dans la décision (arête vive), mais une droite.
- minimiser l'erreur tout en évitant d'êtres bloqué dans des minimums locaux.

Problème : on ne peut avoir que des décisions binaires, linéaires -> Sigmoid activation function.

```text
yj = 1/(1+e^-Sj), S=sum(wixi)
deltawj = n(d-y)yj'xj <-- dérivée
yj'=yj(1-yj)
```

```text
x=(1,n), w=(1,-1), b=0

S=(w^t)x+b = 1 - 1 + 0 = 0
y = 1/(1+e^) = 1/(1+e^0) = 1/2 = 0.5
y(1-y) = 1/2 * 1/2 = 1/4 = 0.25

deltaw = n(d-y)yj'(1-yj)xj = 0.2*0.5*0.25*(1,1)=(0.025,0.025)
```

```text
s = w^t * x + b = 4
sigmoid(4) =~ 0.981
d=1
n=0.2
x=(1,1)

y=1/1+e^-s = 0.981
y(1-y)= 0.0177
d-y = 1-0.981 = 0.018
deltaw = n(d-y)y(1-y)x = 0.2*0.018*0.0177*(1,1) = 0.000063
```

--> deltaw très faible, donc très lent et petit pas pour update les poids. (fading gradient)

exo: trouver 3 inégalités linéaires pour former un triangle entre (0,1), (1,0) et (0,0)

## Chapitre 4

On donne du momentum à la backpropagation pour que la balle puisse sortir des minimum locaux.

- learning rate = vitesse constante de la balle
- momentum = à quel vitesse elle gagne et perd de la vitesse

### Gradient Descent / Stochastic Descent / Mini-batch gradient descent

- Gradient Descent: déterministe, linéaire
- Stochastic Descent: aléatoire jusqu'à-ce qu'on trouve le min.
- Mini-batch gradient descent: à partir d'un dataset de training, on shuffle et on crée des mini-batches. Ensuite, on fait la moyenne du gradient descent pour chacun jusqu'à ce qu'on ait une epoch.

### Optimizers

- Stochastic Gradient Descent SGD: minibatch backprop with momentum
- Root Mean Square Propagation RMSprop: useful to establish a baseline performance
- Adaptive Moment estimation Adam: on prend en compte la dérivée actuelle et la dernière dérivée calculée, on fait la moyenne -> lisser, avoir moins de variation

## Parameters

- Topologie: nb layers, nb neurons dans chaque hidden layer
- Activity function: sigmoid, tanh, exp, ReLu

Une fonction d'activation par couche -> dernière couche a souvent sigmoid, couches cachées ont autres.

- Weight Initialisation: small = all same, high = saturation
- Learning Rate: vitesse
- Input normalization: si les inputs ont peu de caractéristiqes individuelles avec lesquelles on peut les différencier
- Encoding categorical data: si on a des données avec deux extrêmes, utiliser i-of-C coding: (1,0) et (0,1)
- Class imbalance problem: 'sil y a des données rares, mauvaise répartition, ... -> échantillonner différemment, si on a une classe minoritaire, essayer d'augmenter leur proportion en en injectant (weighted loss training)
- Loss: Classification = cross-entropy, Regression = mse ()

## Overfitting

si on a trop de neurones, ça peut créer une fonction qui est super bizarre et précise -> overfitting, on peut distinguer quasiment chacun des points.

```text
-> training error descend, validation error augmente -> tout mémorisé et rien deviné
```

- Biais: erreur systématique du modèle (1/nombre de neurones)
- Variance: sensibilité du modèle (nombre de neurones)

### Pour fixer ça

- Early stopping: stopper avant l'overfitting, quand on détecte que la validation error arrête de s'améliorer
- Regularization: pénaliser certains paramètres, p.ex proportionnelle à la taille des poids pour pénaliser les gros poids.
- Data augmentation: améliorer les données, en avoir plus

## Chapitre 5

Data normalization: toutes les données entre 0 et 1, ou -1 et 1:

```text
x' = (x-xmin)/(xmax-xmin)
```

!! Outliers: données compressées --> rescaling: capping values

Weight initialization: random initial values. zero -> all the same

### Learning rate

- very high: part en vrille
- low: très lent
- high: minimum local
- optimal: minimum global

pour trouver ça, c'est pas mal empirique: AutoML (ajuste automatiquement hyperparams), KerasTuner (tuning de params), GridSearchCV & RandomizedSearchCV (recherches systématiques -> grid ou params aléatoire)

## Underfitting / Overfitting / Good fit

- Underfitting: on arrive pas à diminuer l'erreur car on a un biais qui est énorme, p.ex un seul neurone, ou juste une droite -> même si on entraine énormément, on a une erreur minimale haute.
- Overfitting: la distance entre l'erreur de validation et de training augmente.
- Good fit: stable, small gap between validation and train

## Unrepresentative training dataset

dataset d'entrainement trop petit ou trop peu diversifié ou biaisé.

## CNN typical architecture

feature maps (matrice A x matrice B), réduction à chaque feature map sur les bords. 9 pixels into 1 pixel
n feature map, peut avoir des gros mismatches.
Convolution layers : 9 x, 9 poids + 1 biais -> into 1 neurone y = sum(wixi) + b

## Maxpooling

reduce  a n:n matrix to a n/m:n/m matrix by taking the max value within m^2 cases.
