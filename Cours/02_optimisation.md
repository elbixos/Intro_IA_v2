<script type="text/javascript" async src="//cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>


## Apprentissage par optimisation

### Principe

La plupart des véritables algorithmes de machine learning sont appartiennent à la catégorie de l'**apprentissage par optimisation**. Optimisation est ici à prendre au sens de "optimisation mathématique" : la recherche du minimum ou du maximum d'une fonction.

*Important* : C'est une idée majeure en ingénierie : si l'on cherche "la meilleure solution à un problème", qu'on peut mesurer la qualité d'une solution (on dispose d'une mesure de performances), et qu'on peut mettre au point un **modèle de solution** dont on peut faire varier les **paramètres**, la recherche de solution consiste en fait à **trouver le jeu de paramètres du modèle qui donne les meilleures performances**. (la proba de classification correcte la plus haute ou la proba d'erreur la plus faible).

*Notre problème d'ingénierie, assez complexe, est ainsi ramené à un problème mathématique classique, pour lequel on dispose de nombreuses solutions.*

### Exemple de mise en oeuvre

Pour vous illustrer cette démarche et sa mise en oeuvre, je vais me servir d'un exemple : On reprend le problème de séparation hommes-femmes, avec comme caractéristiques : *[taille, poids]*.

Je veux construire un algorithme qui va **trouver la meilleure droite pour séparer ces deux classes**. Pour simplifier le problème, je vais supposer que je sais de plus que cette droite passe par un point M, fixé par avance.

Voici ma base d'apprentissage et le point M (marqué en noir) :

![nuage de point et M](images\hommesFemmesPoint.png)

L'équation de n'importe quelle droite (non verticale) passant par ce point $$(M[0],M[1])$$
est donnée par : $$\frac{y - M[1]}{x-M[0]} = p $$, avec $$p \in \mathbb{R}$$

en bricolant un peu cette équation, on passe successivement à :
$$y - M[1] = p.(x-M[0]$$, ou encore : $$y - M[1] - p.(x-M[0]) = 0$$ 

Pour un paramètre p fixé (donc pour une droite donnée), si le vecteur de caractéristiques est $$[x,y]$$, on peut donc calculer la quantité $$y - M[1] - p.(x-M[0])$$. Le signe nous indique si le point est au dessus (>0) ou en dessous de la droite (<0).

La décision de notre algorithme est prise de la façon suivante :
- Si $$y - p(x-M[0]) -M[1] > 0$$ : on décide que c'est un homme.
- Sinon, c'est une femme.

(le cas ou la valeur est nulle signifie qu'on pourrait prendre n'importe laquelle des deux décisions. Ici, j'ai choisi arbitrairement des femmes dans ce cas).

La figure suivante présente deux exemples de droites (donc deux algorithmes différents mais basés sur ce principe), dont les performances en apprentissage sont différentes. 


![deux droites de séparation](images\hommesFemmesDroites.png)

Posez-vous les questions suivantes :

1. Entre ces deux algorithmes, lequel est le meilleur ?
2. Peut-on faire mieux ?

Voici les réponses, et ce qu'elles nous indiquerons pour la modelisation du problème :

1. Le meilleur algorithme est le jaune, car il commet moins d'erreurs, en particulier sur les femmes à gauche du graphique. Je peux ainsi chercher à trouver la droite qui minimise le nombre d'erreurs commises en moyenne sur la base d'apprentissage. Ce nombre d'erreur est alors ma fonction de **mesure de performances**, dont la valeur dépend de la droite testée.
2. Pour faire mieux il faudrait continuer à augmenter la pente pour réduire les erreurs sur les hommes en bas à droite. La pente de la droite est le **paramètre du modèle**

Trouver la meilleure droite consiste ainsi à trouver le paramètre $$p$$ qui donne les meilleurs performances sur l'ensemble des exemples d'apprentissage.

Comme annoncé au début de cette section, « meilleure » est ici défini de façon très précise : c'est le paramètre qui **permet d'obtenir la meilleure mesure de performance en apprentissage**. C'est un problème d'**optimisation** (au sens mathématique : trouver la valeur qui maximise ou minimise une fonction).

Si vous avez eu des cours à ce sujet, vous connaissez peut être des solutions à ce type de problèmes. Sinon, pas de panique, voyons déja une solution simple à mettre en oeuvre

### Premier algorithme d'apprentissage par optimisation

La solution la plus simple pour trouver une « bonne » solution consiste à :

1. Partir d'un jeu de paramètres fixé (une valeur initiale pour la pente de la droite) ;
2. Évaluer les performances de ce jeu de paramètre (en mesurant sa proba d'erreur par exemple) :
3. Modifier au hasard ce (ou ces) paramètre(s) en observant si la modification améliore les performances ;
4. Si une amélioration est constatée, on s'en sert comme nouveau point de départ ;
5. On recommence en 3.

Une version pseudo code de cet algorithme figure ci-dessous :

```python
# On se donne une situation de départ
testp=-50
# On crée une variable pour mémoriser les meilleurs performances trouvées
bestPerreur = 1
# On crée une variable pour fixer le nombre d'essais maximum
nMax= 1000
# On crée une variable pour compter les tests déjà faits
# Puis on va chercher de meilleures solutions.
while n < nMax:
  # On mesure les perf obtenues sur la base d'apprentissage avec ces paramètres.
  newPerreur = mesureProbaErreur(testp, baseApprentissage)
  # On met à jour les meilleurs perf trouvées et les meilleurs param trouvés
  if newPerreur < bestPerreur :
    p = testp
    bestPerreur = newPerreur
  # On génère une nouvelle configuration en déplaçant testp # UN PETIT PEU et AU HASARD !
  testp+= random.random() *pas
  n+=1
```

Mon pseudo code fonctionne en Python (sous réserve d'avoir fait la fonction *mesureProbaErreur*). À ceci près que je n'ai pas fixé de valeur à la variable *pas*... Cette variable s'appelle  « le **pas de la descente** ».

À l'issue de cet algorithme :

- on aura testé 1000 possibilités pour p
- en partant de p=-50
- en conservant toujours la meilleur possible
- en essayant toujours juste à coté de la meilleure solution trouvée jusque-là

Cet algorithme, relativement simple mais efficace, rentre dans la grande catégorie des algorithmes d'optimisation qui servent à peu près à tout. Notamment, ils peuvent servir à **trouver les poids d'un réseau de neurones**, réseaux que nous verrons dans le prochain chapitre. Le paramètre **pas de la descente** évoqué dans notre pseudo code se retrouve dans toutes ces méthodes, sous une forme ou une autre. Il faut souvent l'adapter au problème traité.

Notre algorithme en particulier utilise une grande part de hasard. Il fait partie de la famille des **algorithmes de descente stochastique**, appelés aussi algorithmes « de **Monte-Carlo** ». Nous verrons des algorithmes plus élaborés dans les pages de niveau 1.

Pour information, voici les résultats trouvés sur ce jeu de test par cet algo :

```
pente : -177.35109477634492
proba d erreur : 0.086
```

![Meilleure droite trouvée](images\hommesFemmesMeilleurDroite.png)

Cet algo fonctionne !

La bonne nouvelle, c'est que vous pouvez aller [le tester](https://colab.research.google.com/drive/18lgzZKC7N9-24rP62h3PlDY_ymPu5MsB). Je vous recommande d'essayer de changer le pas, le nombre d'itérations nMax pour voir ce que cela change (et de comprendre pourquoi)...

#### Recherche de plus d'un paramètre

Le mode de fonctionnement est très similaire si **l'algorithme travaille avec plus d'un paramètre**.

Imaginons que je ne sache pas placer le point $$M$$. Les droites que je cherche maintenant ont pour équation par $$y - px -b = 0$$. On cherche donc maintenant le meilleur couple $$(p,b)$$. Sauriez vous adapter l'algo précédent ?

Réponse : Il suffirait de changer :

- la description de la configuration initiale : on choisit un $$p$$ et un $$b$$ initiaux, 
- la génération de nouvelle configuration : on modifie $$p$$ et $$b$$ un petit peu, à partir de la meilleure solution trouvée jusque là

#### Complexité du modèle

Nous l'avons vu, cet algorithme recherche "la meilleure droite" pour séparer les classes. Nous aurions pu utiliser un modèle plus complexe pour trouver "le meilleur polynome de degré 2" ou toute autre modèle plus complexe, potentiellement capable de mieux séparer nos deux classes.

*Important* : **Il revient au développeur de choisir un modèle capable de s'adapter à ses données**.

Dans l'exemple qui suit, les données ne sont vraiment pas adaptées à un modèle de séparation linéaire (par une droite), qui quel que soit le jeu de paramètre choisi, n'obtiendra pas de bonnes performances.

![classes non linéairement séparables](images\classes_non_sepa_lin.png)

Notez qu'il peut arriver que, quel que soit le modèle, il soit simplement impossible d'obtenir des performances correctes pour un problème donné. C'est le cas par si les caractéristiques ne permettent pas de prendre une décision correcte. Ceci est illustré dans la figure suivante, présentant un problème de classification très ardu. Dans ce problème, les exemples des deux classes se superposent beaucoup. Quel que soit le modèle, il fera nécessairement un nombre d'erreurs important.

Formellement, si plus les distributions des caractéristiques de chaque classe se superposent, plus le problème est difficile.

![problème difficile](images\pb_difficile.png)

#### Évolution des performances pendant l'apprentissage

On voit dans le pseudo code qui précède, que l'algo va en fait voir la totalité de la base de nombreuses fois (1000 fois).

Au fur et à mesure des essais, la **probabilité d'erreur devrait diminuer**. Dit autrement, la **précision en apprentissage** doit augmenter avec le temps, pendant l'apprentissage. Elle doivent (si tout se passe bien) évoluer de la façon suivante :


![perf durant l'apprentissage](images\scalarFMnistMono.png)

La figure précédente est en fait tirée de tests effectués sur de la reconnaissance de chiffres à partir d'images (à l'aide d'un réseau de neurones). Dans ce problème, il y a 10 classes possibles. L'algorithme aurait donc, en tirant totalement au hasard sa réponse, une probabilité de succès de 0.1. Ce sont les performances au début de l'apprentissage. On note très clairement qu'il progresse, puis stagne.

On voit que notre algorithme a atteint, à la fin de son apprentissage, une précision en apprentissage d'à peu près 0.80. La valeur finale est peu importante (elle dépend de la qualité de l'algorithme et de la difficulté du problème). Par contre, on voit qu'il ne sert plus à rien de continuer à entraîner cet algorithme, il a déjà atteint ses limites depuis longtemps.

Je vous rappelle que **ces performances ne sont toutefois pas une évaluation correcte de ce que ferait notre algorithme en situations réelles**, car il est ici évalué sur des exemples qu'il connaît déjà. On doit donc mesurer maintenant sa probabilité de classification correcte sur la base de généralisation.

Dans le cas correspondant à l'image présentée au-dessus, **la probabilité de classification correcte en généralisation était de 0.76**. Qu'en déduisez-vous ?

(Réponse : pas d'overfitting, il faudra **améliorer le modèle** pour espérer avoir de meilleures performances.)

### A retenir

Durant ce chapitre, nous avons vu de nombreuses notions primordiales concernant les techniques d'apprentissage automatique. Je vous en fais ici le résumé rapide. J'en profiterais pour ajouter encore un tout petit peu de vocabulaire qui sera utile plus loin.

Pour trouver un algorithme efficace, l'apprentissage par optimisation procède comme suit :

- on se donne une **mesure de performance** des algorithmes trouvés (par exemple la probabilité d'erreur).
- on se donne un **modèle**, adaptable aux données en fonction de ses **paramètres** (par exemple, une droite)
- En partant d'un **jeu de paramètres initial**, un **algorithme d'optimisation** va chercher le jeu de paramètre permettant d'obtenir **les performances les meilleures sur la base d'apprentissage**. Ceci peut nécessiter de nombreuses itérations sur la base. En fin d'apprentissage, le jeu de paramètres adéquat est supposé trouvé.

Si l'algorithme obtient des performances médiocres sur la base d'apprentissage, il est possible que le modèle ne soit pas assez complexe (auquel cas il faut changer de modèle). On dit alors que le modèle est en situation de **sous-apprentissage**. Il peut arriver aussi que la difficulté intrinsèque du problème soit trop grande (auquel cas, on est foutus !).

Si l'algorithme obtient des performances acceptables sur la base d'apprentissage, on doit évaluer ses performances sur la base de généralisation. Il peut arriver que l'algorithme obtienne de très bonnes performances sur la base d'apprentissage, mais médiocres sur la base de généralisation. L'algorithme est alors en situation de **sur-apprentissage**.

Enfin, notons un nouveau terme pour la suite :

*Définition* : Lors de l'apprentissage, il est de coutume d'utiliser une mesure de performance que l'on veut minimiser (comme la probabilité d'erreur). Cette mesure de performance est alors appelée **Fonction de Coût** (ou **loss function** en anglais).

L'apprentissage par optimisation consiste donc le plus souvent à trouver, à l'aide d'un algorithme d'optimisation, le jeu de paramètres de notre modèle permettant de **minimiser le coût**, sur l'ensemble des exemples de la base d'apprentissage.

Dans les pages de niveau 1 et 2, nous aurons l'occasion de définir d'autres fonctions de coûts, plus efficaces que la probabilité d'erreur. Nous verrons également des algorithmes d'optimisation plus rapides ou plus efficaces que notre descente stochastique simple.

Enfin, nous aurons également l'occasion d'étudier des modèles plus complexes que notre simple droite, ce que nous ferons dans le prochain chapitre, qui traitera des réseaux de neurones artificiels.
