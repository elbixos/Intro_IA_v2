
## Introduction aux Réseaux de Neurones.

### Introduction

Ne nous le cachons pas, lorsque l'on pense aux techniques d'apprentissage automatique (machine learning), ce sont actuellement surtout aux Réseaux de Neurones Artificiels auxquels on fait référence. Quand on parle de **Classification** ou de **Régression** (pas trop pour le clustering), ils ont, à proprement parler, **écrasé toutes les autres techniques**.

Il faut noter que **ce ne sera vraisemblablement pas toujours le cas**. Il y a eu des modes en machine learning (nous verrons ces notions dans les pages de niveau 1 et 2 mais historiquement,la *théorie bayésienne* du début a cédé la place aux *arbres de décision* qui ont été parfois supplantés par les *Support Vector Machines*...). Maintenant, ce sont les réseaux de neurones qui dominent le domaine de l'Apprentissage Automatique. Pourtant, ces réseaux existent depuis 1960 !

Ce qui a changé et a permis leur domination absolue actuelle, c'est une conjonction des facteurs suivants :

- des développements récents (pas si récents) : les réseaux convolutionnels, ...
- une augmentation énorme des puissances de calcul, 
- la possibilité d'accéder à des bases de données énormes et nombreuses (par exemple, www.kaggle.com, mais je ne suis pas là pour leur faire de la pub), 
- des **frameworks de développement** très matures (*Tensor Flow* ou *Torch*, ...) qui rendent leur programmation suffisamment aisée pour que les entreprises puissent envisager de les utiliser sans avoir besoin d'un réel spécialiste (dans un premier temps).

Il faut tout de même noter que cette nouvelle mode se distingue des précédentes par un point majeur que nous allons voir maintenant.

### Ampleur des améliorations obtenues avec eux

L'ampleur de l'amélioration qu'ils ont amenés (dans leurs formes récentes) est sans commune mesure avec les améliorations précédentes. Avant, une amélioration permettait de gagner quelques points sur le pourcentage de décisions correctes. Les réseaux de neurones ont tellement bien réussi qu'il est difficile de les départager sur les anciennes bases d'exemples (sur la base MNIST, les versions élaborées performent toutes avec une proba de classification correcte au dessus de 0.98, par exemple).

Il a fallu créer **de nouvelles bases** constituant des problèmes plus difficiles. Par exemple, un problème classique, avant, était la reconnaissance d'un chiffre manuscrit (10 classes possibles) dans des images de taille identiques (28x28 pixels). C'était la base MNIST. Un problème plus actuel correspondrait à la base Image.Net dans laquelle il faut reconnaître quels objets apparaissent (1000 classes possibles) dans des images de taille variable. Je n'ai pas connaissance d'algo autre que les réseaux de neurones pouvant s'attaquer à cela.

Sans forcément parler de révolution *(seul l'avenir nous le dira, à l'échelle d'une vie humaine)*, nous sommes donc en train de vivre un saut technologique très net.

### Présentation des Réseaux de Neurones Artificiels.

Dans cette partie, nous verrons les éléments de base de tout ce que l'on nomme « Deep Neural Networks » (Réseaux de Neurones Profonds), qui nous permettront de mettre au point des **réseaux de neurones denses**.

Quoique les modèles de neurone et les modèles de réseaux de neurones présentés ci-dessous s'inspirent de la biologie (c'est vrai, mais ces modèles seraient simplistes s'il s'agissait de simuler des neurones biologiques et leurs réseaux), ce qui nous intéresse dans ce cours, c'est la **capacité de nos modèles à apprendre** à partir d'exemples. Voici donc comment ces réseaux de neurones artificiels fonctionnent.

Au cœur de ces réseaux, il y a ... des Neurones artificiels, dont voici les principes.

### Le neurone artificiel classique

Un neurone standard en informatique prend des **valeurs numériques en entrée**. Notons $$x$$ ce vecteur à \(N\) composantes : $$ x = [x_1, ... x_N] $$.

Ce sont les informations que reçoit le neurone (ce sont nécessairement des nombres). On peut ainsi imaginer que notre neurone travaille sur des individus, et pour chaque individu, $$x_1$$ soit son âge, $$x_2$$ soit son poids, ...

Le neurone calcule une **sortie** (un nombre réel) en fonction **des entrées et de paramètres internes**. Ces paramètres internes sont les **poids du neurone**. Nous les noterons $$w_i$$. Ils sont au nombre de $$N+1$$ (nous allons voir pourquoi ci-dessous). Ces poids sont également des nombres, et leurs valeurs sont spécifiques à chaque neurone. Initialement (avant apprentissage), **les valeurs de ces poids sont tirées aléatoirement**.

on note l'ensemble de ces poids comme suit : $$ \{w_i , i \in \{0,...n\}\} $$

C'est bien une petite unité de calcul élémentaire dont la puissance viendra de sa mise en réseau.

Son fonctionnement est décrit dans la figure suivante :

![principe neurone](images\neurone.png)

À gauche figurent les N **entrées du neurone**.

En fonction des valeurs observées sur les entrées, le neurone calcule alors la **somme des entrées, pondérées par leurs poids respectifs**. L'expression en est la suivante : 

$$ S = \sum_{i =1..n} w_i.x_i + w_0 $$

*Remarque*
Si vous regardez bien l'équation précédente, vous voyez qu'il y a un poids $$w_0** qui n'est associé à aucune entrée, mais à une valeur fixe $$1$$. On l'appelle le « **biais du neurone** ». Dans la pratique, c'est pris en compte automatiquement. Il permet de **décaler arbitrairement la valeur de sortie du neurone lorsqu'on lui présente un vecteur d'entrée nulle** (comme l'ordonnée à l'origine d'une droite).

Au début des réseaux de neurones (1960), les neurones fonctionnaient comme cela. Mais, dans ce cas, le neurone ne fait qu'une **combinaison linéaire de ses entrées**, ce qui lui donne des capacités limitées. Il est très rapidement apparu très utile d'ajouter au neurone un étage : sa **fonction d'activation**. La fonction d'activation est une fonction non linéaire qui prend en entrée la somme pondérée et la transforme.

Une fonction courante pour cela est la fonction **ReLU**, dont voici le graphe :

![la fonction relu](images\relu.png)

La sortie de la fonction d'activation est la **sortie du neurone** $$(y)$$, on a donc la formule suivante pour le calcul de la sortie d'un neurone :

$$ y = f(\sum_{i =1..N} w_i.x_i + w_0) $$

Dans le cas de la fonction ReLU, le fonctionnement du neurone est donc le suivant :

1. Le neurone calcule la somme pondérée des entrées S.
2. Le passage de cette somme S dans la fonction d'activation donne la sortie y du neurone :
    - Si $$S>0$$, alors $$y = S$$
    - Sinon $$y = 0$$

Il existe de nombreuses autres fonctions d'activations. Éventuellement, nous y reviendrons. Elles obéissent toutes plus ou moins (sauf cas très spécifique) à l'idée suivante : « **Plus la somme pondérée des entrées est grande, plus la sortie est grande.** »

L'idée derrière tout ces calculs est la suivante : un neurone va synthétiser les informations qui lui parviennent (ses entrées), de façon personnalisée (en fonctions de *ses* poids). Il décidera alors de réagir avec une sortie plus ou moins grande.

Voyons maintenant comment utiliser ces neurones pour une tâche de classification.

### Le réseau de neurones à couches en classification

Un **réseau de neurones** consiste simplement en un ensemble de neurones à connectés entre eux. La sortie d'un neurone constitue une entrée pour un ou plusieurs autres neurones.

Le réseau de neurones le plus classique, acyclique et dense (**FeedForward dense**) est construit avec une **architecture** en **couches**.

Comme il sera souvent appliqué à des problèmes de classification, une architecture dédiée à ce type de problème est présentée dans la figure suivante :

![archi DNN Classif](images\DNN.png)

On trouve dans cette architecture 3 types de couches différentes :

- La **couche d'entrée**, qui reçoit les données (les caractéristiques de l'objet à classifier) ;
- Aucune, une ou plusieurs **couches cachées**, qui permettent de traiter les données ;
- La **couche de sortie** qui présente **autant de neurones que le problème a de classes**.

Lorsque les neurones ont une entrée connectée à la sortie de chaque neurone de la couche précédente, on dit que « **le réseau est dense** ». C'est le modèle le plus courant que vous rencontrerez dans un premier temps.

#### Enchainement des calculs dans le réseau (en inférence)
Imaginons un réseau de neurones dense, **déja entrainé**.
Reprenons alors l'enchaînement des opérations effectuées par le réseau quand on lui présente un exemple de la base de données (en **inférence**) :

1. Le vecteur de caractéristiques de cet exemple est placé sur la couche d'entrée.
2. Chaque neurone de la couche 1 calcule ses sorties en fonction de ses poids et de ces entrées.
3. Ces sorties sont communiquées à la couche 2, qui procède de la même façon.
4. Les calculs se poursuivent ainsi jusqu'à la couche de sortie.

#### Prise de décision

Comme dit précédemment, la couche de sortie possède autant de neurones que le problème a de classes possibles. **Chaque neurone de la couche de sortie est en charge de reconnaître une classe spécifique**.

Ainsi, quand on communique des caractéristiques à un réseau de neurone, chaque neurone de sortie calcule **un score pour la classe correspondante**. La décision du réseau pour un exemple donnée est prise en regardant quel neurone de sortie a la sortie la plus grande.

Reprenons cela sous forme plus mathématique :

Considérons qu'on présente un exemple, de caractéristiques $$x$$. Le réseau calcule successivement les sorties de chaque couche, jusqu'à obtention des sorties de la dernière couche. Si l'on note $$y_j$$ les sorties de chaque neurone de la couche de sortie, celle ci fournit donc un vecteur $$y$$ de dimension égale au nombre de classes du problème.

La décision du réseau pour cet exemple est la classe numéro $$d$$, avec $$ d = argmax_{i} (y_i) $$

### Les poids du réseau de neurones : exemple d'application

Les poids de l'ensemble des neurones du réseau sont primordiaux puisque ce sont eux qui déterminent les sorties du réseau pour une entrée particulière. Chaque neurone a autant de poids qu'il a d'entrées, plus une (pour le biais). Ces poids sont les **paramètres libres** de notre réseau, qu'il faudra **régler pendant la phase d'apprentissage**.

Par exemple, imaginons que vous vouliez classifier des individus selon leur sexe, en fonction de leur taille et de leur poids.

- Notre réseau aura 2 entrées : une pour la taille, une pour le poids ; 
- Notre réseau aura 2 sorties : une pour les hommes, une pour les femmes ;
- Je décide de façon arbitraire de mettre entre les deux 2 couches cachées, de 5 neurones chacune.

Une représentation sommaire de ce réseau est la suivante :

![modele simple DNN classif](images\modelSimpleDNN.png)

On retrouve dans cette image toute notre architecture :

- Sur la couche d'entrée, on a bien 2 informations par exemple présenté (poids, taille). Le « ? » signifie qu'on pourrait envoyer plusieurs exemples en même temps, qui seront traités de façon parallèle. Notez que **la couche d'entrée ne contient pas de neurones**. C'est juste le lieu où l'on place les caractéristiques... Elle a donc 2 sorties pour chaque exemple (poids et taille)
- La première couche cachée a donc bien 2 entrées (pour chaque exemple) et cette couche produit bien en sortie 5 informations (la sortie de chacun de ses 5 neurones) ; 
- La seconde couche cachée est composée de neurones ayant tous 5 informations en entrée, et produisant en sortie 5 informations (il y a 5 neurones aussi dans cette couche) ;
- La couche de sortie a bien 5 entrées, et 2 sorties, correspondant au nombre de classes du problème.

Pour s'assurer que tout est bien compris, voyons quel est le nombre de paramètres libres de notre réseau :

- Première couche : chaque neurone a 2 entrées et un biais, soit  5.(2+1) = 15 poids au total ;
- Seconde couche (complètement connectée à la première) : chaque neurone a 5 entrées (autant que de neurones sur la première couche), soit 5. (5+1) = 30 poids au total ;
- Couche de sortie : chaque neurone a 5 entrées, soit 2.(5+1) = 12 poids au total.

Soit un total de $$15+30+12=57$$ **paramètres libres** pour notre réseau. Ce nombre est important pour la **capacité d'apprentissage** du réseau. Trop faible, le réseau sera en situation de **sous-apprentissage**. Trop grand, le réseau risque de faire du **sur-apprentissage** (nous reviendrons sur ces notions plus loin).

Comme annoncé auparavant tous ces poids sont **initialement fixés aléatoirement**. Le réseau **prend donc ses décisions initialement au hasard**. Il va donc falloir que le réseau règle ses poids lors de l'apprentissage, ce que nous allons voir maintenant.

### Apprentissage supervisé des poids

Pour que le réseau adapte ses poids au problème traité, il faut procéder à la phase d'apprentissage. Elle nécessite de disposer de nombreux exemples, réunis dans la base d'apprentissage. Pour chaque exemple, on dispose :

- des **caractéristiques** de l'exemple
- du **label** : la réponse attendue pour cet exemple (un numéro allant de 0 à $$m-1$$, si $$m$$ est le nombre de classes possibles).

La **phase d'apprentissage** va consister à réitérer la séquence suivante de nombreuses fois :

- lui présenter des exemples,
- lui faire calculer sa réponse (le numéro du neurone de sortie ayant donné la sortie la plus forte) et les comparer aux labels attendus,
- puis **corriger ses poids** de façon à améliorer sa réponse la prochaine fois qu'il verra ces exemples.

Lors de cette correction, on doit corriger les poids en entrée de la couche de sortie, mais aussi les poids entre toutes les couches cachées, jusqu'à la couche d'entrée. La détermination des modifications vers l'arrière du réseau se nomme  « la **rétropropagation du gradient d'erreur** » (**backpropagation** en anglais).

Pour chaque exemple (parfois, on présente plusieurs exemples en parallèle et l'on calcule la modification globale à effectuer), la correction **améliorera légèrement le résultat du neurone**, si on lui présentait le même exemple. Pour des questions de stabilité, on n'effectue pas une correction parfaite de la réponse du neurone à cet exemple.

On présente ainsi successivement tous les exemples de la base d'apprentissage. Une passe complète de la base d'apprentissage s'appelle une « **epoch** ». Il faudra enchaîner de nombreuses epoch pour atteindre des performances correctes.

Notre apprentissage consiste donc à trouver les meilleurs poids possibles pour la base d'apprentissage, ce que, nous l'avons vu auparavant, on peut traduire en :  "c'est un problème d'**optimisation de paramètres**". Celle-ci est faite par un algorithme de descente stochastique (semblable à notre algorithme d'optimisation simple vu dans le chapitre précédent, mais beaucoup plus efficace), à partir d'une situation initiale prise au hasard.

À l'issue de l'apprentissage, les poids sont considérés comme appris, et comme pour tout algo d'apprentissage supervisé, on évaluera ses performances à l'aide d'une **base de validation**.

Ce qui précède décrit l'essentiel du principe de fonctionnement des réseaux de neurones **feedforward (en couches) denses**. Il me faudra néanmoins revenir sur quelques notions pour la mise en pratique. En particulier, je serai plus précis sur la notion de calcul de performances, liées à la fonction de perte dans la partie suivante.

Par ailleurs, si vous voulez avoir plus de détail sur les algorithmes d'optimisation, il y a une sous section dédiée dans les pages de niveau 1 et 2. Certains trouveront sans doute intéressant d'en savoir plus sur les mathématiques impliquées dans l'apprentissage des réseaux de neurones.

