<script type="text/javascript" async src="//cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

## Algorithmes de classification

### Introduction

Au cours de notre périple concernant l'apprentissage automatique et les
problèmes de classification, nous avons déjà eu l'occasion de croiser
l'algorithme du **plus proche voisin** (ou ppv), et sa variante des
**k plus proches voisins** (ou kppv ou encore knn pour les anglophones).
Je ne reviendrais pas dessus, leur principe n'ayant pas dû vous poser trop de
problème.

Je vous ai également présenté de façon relativement rapide (je complèterais
plus loin) les **réseaux de neurones denses** qui ont ces derniers temps les
performances les meilleures (pour peu qu'on dispose de suffisamment d'exemples,
et qu'on sache trouver une architecture adaptée au problème).

Dans cette section, je vais vous présenter quelques algorithmes, plus anciens,
qui ont eu leur heure de gloire pour traiter les problèmes de classification.
L'objectif est double :

- D'une part, il s'agit de **vous constituer une certaine culture** du
    *machine learning.*
- D'autre part, il peut être utile de disposer de ces outils, soit pour
**se faire une idée des performances** qu'on peut atteindre sur un problème
précis, soit parce que la base d'exemples s'y prête.
Par exemple, si elle est petite, il sera peut être compliqué pour un réseau de
neurone d'apprendre dessus.

Nous verrons donc par la suite :

- les algorithmes basés sur la **théorie de la décision Bayésienne**,
- **les arbres de décision**
- et les **Support Vector Machines** (ou SVM), que nous avons déjà utilisés sans vraiment détailler leur fonctionnement.

### Thérie bayesienne de la décision

C'est la théorie la plus ancienne, venue d'un temps où l'on ne disposait que des proba stats pour envisager de répondre à un problème de classification. On lui doit notamment d'avoir fixé une grande partie du vocabulaire du machine learning, ainsi qu'un certain nombre de notions intéressantes. Voyons ceci rapidement.

Soit un problème de classification, dans lequel, pour un vecteur de
caractéristiques $$x$$, je dois prendre une décision parmi $$m$$ classes
possibles.
L'ensemble des classes possibles pourra être noté
$$\left\{ h_i, i \in [1..m] \right\}$$.

La vision probabiliste de ce problème consistera à considérer que le vecteur des
caractéristiques de notre exemple $$x$$ est une **réalisation** d'une
**Variable Aléatoire** $$X$$.
De même, la classe de notre exemple sera considérée comme une réalisation
d'une variable aléatoire $$H$$.

Pour illustrer mes calculs, je prendrais pour commencer comme exemple un $$X$$
de dimension 1 : la taille des individus.
$$x$$, la taille observée d'un exemple, pourra valoir, par exemple, 1.62m.
Les classes possibles seront « homme » et « femme ».

#### Les calculs

Ceci posé, on peut s'intéresser à la quantité suivante :

$$f_i(x) = P(H = h_i / X =x)$$ Cette quantité correspond à la probabilité d'avoir
la classe $$h_i$$, sachant que le vecteur de caractéristiques est $$x$$.
Appliqué à notre exemple, ce pourrait être la
**probabilité d'être un homme quand on mesure 1.62m**.

**Idée de base de la Théorie Bayésienne de la décision** :
De fait, on peut montrer que si l'on souhaite se tromper le moins souvent
possible (en moyenne), il faut, pour un $$x$$ donné, calculer cette quantité
$$f_i(x)$$ pour chaque classe possible, et choisir la classe pour laquelle
elle est la plus grande.

Il n'y a pas besoin de grandes démonstrations :
**si l'on prend la classe la plus probable, on se trompe moins souvent en moyenne.**

Notez tout de même l'élégance du procédé. Reste à calculer cette quantité, ce qui est plus retors.

De plus, pour la suite, pour simplifier les notations, je vais adopter une **notation d'ingénieur**, qui fait hurler les mathématiciens (mais qui est beaucoup plus concise).

**Notation Rapide** : Au lieu d'écrire $$ P( H=h_i, X=x) $$, par exemple, j'écrirais simplement $$P(h_i / x)$$.

##### Rappels de probabilité : définitions

Avec ces notations rapides, et avant d'aller plus loin, voici quelques rappels de proba sur les variables aléatoires conjointes. Pour les puristes, notez que je ne distingue pas ici probabilité et densité de probabilité (cela qui ne change rien aux calculs théoriques).

- $$P(h_i / x)$$, on l'a dit est la probabilité d'avoir $$h_i$$, **sachant** que le vecteur de caractéristique est \(x\).
- $$P(h_i) $$, serait la probabilité d'avoir $$h_i$$.
Par exemple, la probabilité dans la population, d'être un homme.
- $$P(x) $$, serait la probabilité d'observer $$x$$.
Par exemple, la probabilité d'observer une taille de 1.62m, au sein de la
population.


Enfin, on a deux autres grandeurs importantes :

- $$P(x / h_i )$$, qui est la probabilité d'observer $$x$$, **sachant** que sa classe est $$h_i$$.
Par exemple, ce serait la probabilité de faire 1.62m quand on est un homme.
- $$P(x , h_i)$$, qui est la probabilité d'observer $$x$$ **et** une classe $$h_i$$.
Par exemple, ce serait la probabilité dans la population d'observer un individu
qui soit un homme et ait une taille de 1.62m.
(les mathématiciens préfèrent la noter $$P(x \cap h_i )$$)

À ce stade, sans entraînement, vous confondrez sans doute toutes ces quantités. Ce n'est pas dramatique, j'essayerais de les expliciter à chaque fois. L'objectif est de vous montrer comment on mène ces calculs, pas forcément que vous sachiez les refaire.

Ceci posé, je vais pouvoir continuer mes calculs...

##### Rappels de probabilité : Théorème de Bayes

Comme cela avait été indiqué, ce que l'on souhaite calculer s'écrit
$$P(h_i / x)$$.
Pour ce fait, on utilise le **théorème de Bayes** qui met en relation les trois
quantités suivantes :

**Théorème de Bayes**, version notation réduite :
$$P(a / b) = P(a , b) / P(b) $$

Appliqué à notre cas, on peut utiliser ce théorème pour en déduire :

$$f_i(x) = P(h_i / x) = P(h_i , x) / P(x)$$

ou encore

$$f_i(x) = P(h_i / x) = P(x / h_i). P(h_i) / P(x) $$

Or si l'on se souvient de l'idée de base de la
**Théorie Bayesienne de la Décision**, il faut, pour un \(x\) donné, calculer
cette quantité pour toutes les classes $$h_i$$ possibles, et prendre celle qui
a donné le résultat le plus grand.
Dans ce contexte, pour un $$x$$ donné, $$P(x)$$ est constant entre les
différentes classes (ce serait, par exemple la probabilité de faire 1.62m).
Pour comparer les résultats obtenus par les différentes classes, on peut simplement supprimer ce terme et ne calculer que la quantité suivante :

$$g_i(x) = P(h_i / x) = P(x / h_i). P(h_i)  $$

Vous pouvez respirer, vous êtes au bout de vos peines :
Chacun de ces termes est relativement simple à estimer.

- $$P(x / h_i)$$ est la probabilité, pour les membres de la classe $$h_i$$
d'avoir la caractéristique $$x$$.
Pour notre cas d'application, c'est la probabilité qu'un homme ait une taille
de 1.62m.
Ceci peut être estimé sur la base d'exemples, ou par une étude préalable.

- $$P(h_i)$$ est la probabilité que la classe $$h_i$$ apparaisse.
Pour notre cas d'application, c'est la probabilité qu'un individu soit un homme.
Ceci aussi peut être estimé sur la base d'exemples, ou par une étude préalable.

Ainsi, la théorie de la décision bayesienne démontre que l'on peut construire un algorithme qui se trompe en moyenne le moins possible, sous réserve que l'on connaisse :

- $$P(h_i)$$ (ça, ça va)
- $$P(x/h_i)$$  Ceci est la partie la plus délicate quand on s'intéresse à des
$$x$$ plus complexes que une simple taille.

Avec ces quantité, pour construire un algorithme optimal, il suffit de
calculer, pour chaque classe :
$$g_i(x) = P(h_i / x) = P(x / h_i). P(h_i)$$ puis de prendre la classe qui
maximise ceci.

##### Algorithme de classification bayésien, lois normales

Imaginons que je doive classifier des hommes et des femmes en fonction de leur taille. C'est assez simple selon cet algorithme.

- Je sais que la densité de proba de taille des hommes et des femmes sont tous deux des gaussiennes.
- J'estime moyenne et écart-type des hommes dans la base d'apprentissage.
Je fais de même avec les femmes.
Ce seront les paramètres $$m_i, \sigma_i$$
($$i=0$$ pour les hommes, $$i=1$$ pour les femmes, par exemple )
- J'estime aussi la proportion d'hommes et de femmes dans ma base
d'apprentissage $$p_i$$
- Ceci me donne la formule de $$g_i(x)$$ pour les hommes et pour les femmes.
$$g_i(x) = p_i \frac{1}{\sqrt{2 \pi \sigma}}  e^{\frac{(x-m_i)^2}{2\sigma_i^2}}$$

Si je dois prendre une décision pour un $$x$$ donné, je calcule ces deux
quantités et je choisis la classe qui a donné le résultat le plus grand.

Un point important est que **sous réserve que mes estimations de moyenne et de variances soient assez bonnes, je ne pourrais pas trouver d'algorithme plus efficace** !

Vous me direz peut-être alors que si c'est si bon, à quoi nous servent les
réseaux de neurones et autres algorithmes compliqués ?
Tout simplement qu'**il n'est pas toujours facile d'estimer correctement**
$$P(x/h_i)$$.
C'est même souvent totalement impossible.

Mais voyons d'abord des cas un peu plus complexes où cet algorithme s'applique.

##### Algorithme de classification bayésien, lois normales multidimensionnelles

Imaginez que $$x$$ soit un vecteur constitué cette fois de
**la taille ET du poids**.
Les calculs précédents restent valables, sauf l'expression de $$P(x/h_i)$$.

Celle-ci représente la probabilité qu'un homme, par exemple, ait une taille de
1.62m ET un poids de 73 kg.

Dans ce cas très précis, on sait que, pour chaque classe, le couple
$$(poids , taille)$$ suit une **loi normale multivariée**, qui est une
extension des gaussiennes aux dimensions supérieures.
Sans entrer dans les détails, cette loi est paramétrée par :

- moyenne de taille
- moyenne des poids
- variance des taille
- variance des poids
- covariance existant entre les deux.

On peut estimer les trois dernières par la matrice de covariance poids taille,
qui est une matrice 2x2.
On dispose ainsi d'une formule pour $$P(x/h_i)$$ et notre algorithme est donc
applicable.

Tant que les caractéristiques suivent une loi normale multivariée, on peut s'en
sortir comme cela.

Ce qui peut arriver, c'est que le nombre d'exemples de la base soit très
faible, et que l'estimation des matrices de covariances soit peu précise.
Dans ce cas, notre algorithme ne fonctionne pas très bien.

Mais on dispose encore éventuellement d'un outil : l'
**indépendance des caractéristiques entre elles**.
Si je dois prédire le salaire d'un individu, en fonction de sa taille et de
son QI, il semble raisonnable de supposer que
**QI et taille sont indépendants**.

$$ P(x/h_i) = P(taille/h_i) . P(QI/h_i) $$ qui utilise l'indépendance entre
taille et QI pour calculer la proba conjointe du couple (taille,QI)

$$P(taille/h_i)$$ et $$(QI/h_i) $$ suivent une loi gaussienne.
Dans ce cas, je n'ai plus besoin d'estimer la covariance taille QI, et obtenir
une estimation plus robuste même si ma base est relativement petite.

Ceci à donné naissance à un algorithme bien connu :

##### Le classifieur bayésien naïf

Si jamais vos caractéristiques ne sont pas indépendantes (par exemple, la
taille et le poids ne sont pas indépendants), ou que vos caractéristiques ne
suivent pas vraiment chacune des gaussiennes (pensez à une distribution de
salaires, par exemple, c'est tout sauf gaussien), les chercheurs en
apprentissage automatique ont eu pendant longtemps un réflexe assez salvateur
dans de nombreux cas : **faire comme si ces conditions étaient vraies**.

Après tout, si les performances ne sont pas trop mauvaises, pourquoi chercher
plus loin ?

C'est le concept d'**algorithme bayésien naïf**, dans lequel les
caractéristiques sont supposées **toutes indépendantes entre elles**, et
**toutes gaussiennes**.

Je vous en parle car il est assez drôle, mais aussi car il peut vous dépanner
grandement dans des cas où la **base est toute petite**, et où les
**caractéristiques sont à peu près gaussiennes**.
Cet algorithme limite en effet l'influence de la malédiction de la
dimensionalité...

Ce classifieur est implémenté en python dans la librairie *sklearn**, par le
module *GaussianNB* (Gaussian Naive Bayes).

##### Conclusion sur ces algorithmes

Extrêmement élégants dans leur formulation, et très efficaces tant que l'on
dispose d'une formule pour calculer $$P(x/h_i)$$, ils ont fait le bonheur de
plusieurs générations.
Ils sont un peu oubliés maintenant que l'on traite des problèmes pour lesquels
les caractéristiques ne sont clairement pas gaussiennes et sont extrêmement
nombreuses...

En revanche, l'idée d’**interpréter les sorties d'un classifieur** comme une
**probabilité d'observer une classe** a infusé vers de nombreux algorithmes
(dont les réseaux de neurones).

C'est vrai à un point que j'estime que si vous ne deviez retenir qu'une chose de
toute cette section, je dirais que c'est ce qui suit :

La **théorie Bayésienne de la décision** tente d'estimer la
**probabilité de chaque classe** compte tenu
**des caractéristiques de l'exemple** traité (et choisit la plus probable).







