<script type="text/javascript" async src="//cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

## Algorithmes

### Interprétation probabiliste des sorties d'un classifieur

Quand on dispose d'un **classifieur** qui associe une classe $$C_i$$ à un
objet, on souhaite souvent savoir avec quelle probabilité il a choisi cette
classe.
C'est un concept que nous avons déjà croisé dans la présentation de la théorie bayésienne de la décision.

Pour un exemple donné de caractéristique $$x$$, on voudrait
**connaître la probabilité que $$x$$ soit de la classe $$C_i$$**
ou, au moins, une estimation par le classifieur de cette quantité.
On noterait cette quantité $$\hat{P}(C_i/x)$$.

Certains algorithmes fournissent spontanément ce type d'informations :

- Les k-nn mesurent le nombre de voisins $$k_i$$ de chaque classe $$C_i$$ 
pour un \(x\) donné.
On a alors $$\hat{P}(C_i/x) = k_i / k$$.
- Un arbre de décision découpe l'espace en régions.
Chaque région contient des exemples de la base d'apprentissage.
La proportion de chaque classe est une estimation de $$\hat{P}(C_i/x)$$.

Dans d'autres cas, en particulier les **réseaux de neurones** (mais pas
seulement), on calcule un **score pour chaque classe**.
La sortie du classifieur pour un problème à $$m$$ classes est en fait un
vecteur $$y$$ de taille $$m$$.
La décision finale est la classe d'indice $$i_{dec} = argmax_i y_i$$

L'idée sous-jacente est donc que **plus une classe $$C_i$$ est probable**,
plus $$y_i$$ est grand.
C'est cette idée qui a conduit à l'utilisation de la fonction Softmax que
nous allons voir maintenant.

#### La fonction softmax

Soit un vecteur de sortie d'un classifieur $$y = [y_1, .. y_m]$$.

Par exemple, dans un problème à trois classes : $$[-2, 0.3, 12.5]$$.
Dans cet exemple, la classe 2 est la plus probable, suivie par la classe 1.
Enfin, la classe 0 est beaucoup moins probable.

La **fonction softmax** va **normaliser** $$y$$ de façon à ce que :

- toutes les **sorties soient positives** et que
- leur **somme vaille 1**.

Cette version normalisée des sorties est assimilable à une probabilité
$$\hat{P}(C_i/x)$$.

Le calcul est relativement simple.

1. On prend l'exponentielle de chaque sortie, pour que nos sorties soient positives.
2. On divise chaque résultat par la somme des éléments, pour que la somme vaille 1.

Calcul de softmax pour une composante :
$$ y\_{soft,k}  = \frac{ e^{y_k} }{ \sum_{i=1..m} e^{y_i} } $$

Dans le cas de l'exemple proposé, $$softmax([-2, 0.3, 12.5])$$ vaut
$$[5e-07 , 5e-06 , 0.9999]$$

Pour prendre un autre exemple $$softmax ([5, 12, 12.5])$$ vaut
$$[3e-04 , 0.37 , 0.62]$$

Ainsi, on peut interpréter ses sorties comme des probabilités d'appartenance à une classe pour tout classifieur fournissant des scores.

Cela sert à l'interprétation humaine des résultats, mais nous servira aussi plus loin pour utiliser l'**entropie croisée** comme fonction de coût des réseaux de neurones.

Notez qu'en python, cette fonction est implémentée par de nombreuses
librairies.
En particulier *scipy.specials.softmax* pour le cas général, mais aussi dans
la librairie *tensorflow* pour les réseaux de neurones puisque la fonction
**softmax peut être utilisée comme fonction d'activation** (de la couche de sortie).

