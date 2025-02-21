<script type="text/javascript" async src="//cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

## Bases d'exemples

### Séparer sa base d'exemples

Pour les applications d'apprentissage supervisé, il arrive fréquemment qu'on dispose d'une unique base d'exemples (la base initiale), définis par leur caractéristiques et munis d'une vérité associée. À partir de ces exemples, nous avons déjà vu qu'il nous faudrait créer au moins 2 bases :

1. la base d'apprentissage, 
2. la base de validation (appelée aussi base de généralisation. je mixerais souvent ces termes).

Il faut aussi définir combien d'exemples feront partie de chaque base.

De façon générale, on mettra quasiment toujours plus d'exemples dans la base d'apprentissage que dans la base de validation. Il faut en effet une **base d'apprentissage aussi grande que possible** pour que l'algorithme puisse extraire des règles de décisions les plus fines possibles. La taille de la base de validation doit être limitée à un minimum permettant de faire une évaluation correcte des performances. La **base de généralisation** doit néanmoins une **taille suffisante pour fournir une variété de cas** représentative des cas réels.

#### Règle ad hoc pour les tailles de base.

Cette règle est **complètement empirique** et concerne la taille relative des 2 bases. Les valeurs de proportion et de taille limite des bases sont purement indicatives. 

##### Petites bases

Pour une base de moins de 1000 exemples, on pourrait choisir quelque chose comme :

- 66% (2/3) des exemples iront en apprentissage 
- 33% (1/3) iront en validation

##### Grandes bases

Pour une base de plus de 100 000 exemples, on pourrait prendre quelque chose comme :

- 90% (9/10) apprentissage
- 10% (1/10) validation

Entre ces deux cas, on pourra utiliser des proportions intermédiaires (3/4 ou 4/5)

Voyons maintenant comment opérer cette séparation.

### Algo de séparation des bases.

Les deux bases doivent avoir des propriétés statistiques semblables même si c'est délicat à expliquer. Par exemple, on ne voudrait pas :

- Qu'une base contienne bcp d'exemples d'une classe alors que l'autre n'en contiendrait que peu ;
- Qu'une base contienne tous les exemples faciles, alors que l'autre contiendrait tous les exemples difficiles.

#### Principe de base

Si l'on dispose de suffisamment d'exemples, et que les classes sont équilibrées, on peut sans problème procéder comme suit :

1. On fixe une proba p d'appartenir à la base d'apprentissage (p=0.66 par exemple)
2. Pour chaque exemple :

    1. on tire un nombre tirage aléatoirement entre 0 et 1.
    2. Si tirage < p, on range l'exemple dans la base d'apprentissage.
    3. Sinon, on range l'exemple dans la base de généralisation.

Ceci garantit que les bases soient à peu près bien formées.

Comme nous l'avons vu dans les Applications Pratiques des pages de base, en Python, la fonction *train_test_split* du module *sklearn.model_selection* permet d'effectuer cette opération très simplement.

#### Principe amélioré

Il peut être important, si certaines classes sont peu représentées, de bien veiller à ce que certaines classes ne disparaissent pas ou presque de l'une ou l'autre des bases. On prendra alors certaines précautions lors des tirages aléatoires pour que ceci ne se produise pas.

Par exemple, on pourrait au préalable :

1. Séparer la base initiale selon les classes des exemples ;
2. Compter le nombre d'exemples total et le nombre d'exemples de chaque classe de la base initiale ;
3. Utiliser p pour établir le nombre d'exemples attendu pour chaque classe, dans chaque base ;
4. Pour chaque classe, on pourra alors choisir aléatoirement dans la base initiale le bon nombre d'exemples d'une classe à affecter à la base d'apprentissage. Les autres iront dans la base de validation.

En Python, la fonction *train_test_split* du module *sklearn.model_selection* permet de choisir ce comportement avancé, avec le paramètre *stratify*.

### Les types de caractéristiques

Ici, on s’intéressera aux différents types de caractéristiques possibles pouvant permettre de composer les vecteurs de caractéristiques. La typologie qui suit vient des proba/stat mais nous l'adopterons au vu des liens qui existent entre cette discipline et l'apprentissage automatique.

On distingue essentiellement deux grandes catégories : les variables **quantitatives** et les variables **qualitatives**.

#### Variables quantitatives

Les **variables quantitatives** sont des informations qui prennent la forme de nombres.

Par exemple, concernant un individu, on peut penser à son nombre d'enfants, sa taille, son âge...

En ce qui nous concerne, c'est parfait, ces nombres peuvent être directement injectés dans un classifieur. Pour le probabiliste, il convient néanmoins de distinguer deux cas : les **variables discrètes** et les **variables continues** car les techniques employées pour les traiter diffèrent. Pour nous, le plus souvent, cela ne change pas grand chose.

##### Les variables quantitatives discrètes

On parle de **variable discrète** lorsque les valeurs possibles sont entières
(ou représentent un ensemble discret, comme $${1.2, 4.3, 4.6, 5.2}$$)

Par exemple, le nombre d'enfants d'un individu. On a rarement 1.3 enfant.

##### Les variables quantitatives continues

On parle de **variable continue** lorsque les valeurs possibles sont réelles.

Par exemple, la taille d'un individu est plutôt une variable continue. L'âge d'un individu est moins clair. C'est théoriquement un nombre réel, mais on ne le définit au mieux qu'au jour près, et le plus souvent, on le donne sous forme d'entier.

#### Variables qualitatives

Les **variables qualitatives** sont des informations qui ne prennent pas la forme de nombres. Elles représentent en général les modalités d'une caractéristique (les catégories possibles d'une caractéristique).

Pour mieux comprendre cette notion, on pourra citer comme exemples la couleur des yeux d'un individu (vert, bleus, marrons,...), ou la taille d'une société (TPE, PME, GE).

Il peut arriver que ces données arrivent sous forme d'une chaîne de caractères, qui ne peut pas être injectée telle quelle dans les classifieurs. Il va donc être nécessaire de les **pré-traiter**. Le prétraitement peut différer en fonction de la **sous catégorie de variable**, selon qu'elle est **ordinale** ou **nominale**.

##### Variables qualitatives ordinales

On parle de **variable qualitative ordinale** lorsque les modalités possibles possèdent la **propriété d'ordre** (qu'ont peut les classer par ordre croissant ou décroissant).

Par exemple, la taille d'une entreprise est clairement ordonnée.

Dans ce cas, on peut éventuellement encoder ces informations sous forme d'un entier comme indiqué dans le tableau suivant.
|:---:|:---:|:---:|:---:|
| Taille de l'entreprise | TPE | PME | GE |
| Encodage | 0 | 1 | 2 |

Il arrive couramment que les bases d'exemples dont on dispose encodent déjà leurs caractéristiques quantitatives avec ce format.

Notez qu'on aurait pu aussi encoder/changer l'échelle de l'encodage pour qu'elle contienne l'information du nombre minimal d'employés dans chaque structure, ce qui donnerait ce qui suit :

|:---:|:---:|:---:|:---:|
| Taille de l'entreprise | TPE | PME | GE |
| Encodage | 1 | 10 | 250 |

Ceci peut être important si la décision de notre algorithme peut bénéficier d'un peu de finesse concernant cette information.

##### Variables qualitatives nominales

On parle de variable qualitative nominale lorsque les modalités possibles ne possèdent pas la propriété d'ordre (qu'on ne peut pas les classer par ordre croissant ou décroissant).

C'est le cas de la couleur des yeux par exemple.

Ce cas des variables qualitatives nominales est assez important pour nous. On pourrait imaginer reprendre l'encodage vu ci-dessus, qui donnerait donc, par exemple :
Couleur des yeux	vert	marron	bleu
Encodage	0	1	2

Ce sera même parfois le cas dans les bases que vous aurez à utiliser.

Néanmoins, cet encodage peut parfois rendre la tâche de nos algorithmes beaucoup plus compliquée.

En effet, un algorithme de classification manipule des chiffres, des distances, des moyennes. Or dans l'absolu, bleu n'est pas deux fois plus que marron, ...

Vous pouvez toujours essayer cet encodage basique. Après tout, si les performances sont satisfaisantes, pourquoi chercher mieux ?

Il est éventuellement préférable d'encoder ces caractéristiques en utilisant la notion de One Hot Vector.

Dans l'encodage One Hot Vector, la caractéristique est remplacée par un vecteur dont la taille correspond au nombre de modalités possibles. Chaque modalité possible est encodée par un vecteur ne comprenant qu'un seul 1.

Par exemple :
couleur des yeux	vert	marron	bleu
Encodage	(1,0,0)	(0,1,0)	(0,0,1)

Ainsi, un algorithme distingue aisément que ces catégories sont très différentes.

Conclusion sur les types de caractéristiques

De fait, cette typologie n'est pas à apprendre par cœur, pour nous. On conservera surtout en tête la différence entre variables quantitatives et qualitatives. Les variables quantitatives doivent être encodées numériquement. On pourra éventuellement s'intéresser à la pertinence d'un encodage One Hot Vector pour certaines caractéristiques qualitatives, si les performances de nos algorithmes restaient décevantes malgré tous nos efforts.