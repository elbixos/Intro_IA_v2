<script type="text/javascript" async src="//cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

## Conclusion des pages de niveau 1

À ce stade, nous avons découvert la majeure partie des concepts en apprentissage automatique.
Pour rappel, je place ci-dessous les notions importantes que nous avons vues
depuis les pages de base.
Si certains points vous semblent flous, n'hésitez pas à retourner à la section
concernée.
Vous devriez être en mesure de lire les deux points suivants en considérant
que chaque point est totalement évident.

- Pour l'**apprentissage supervisé**, il est nécessaire de disposer d'une
**base d'apprentissage** et d'une **base de généralisation**.
Il peut être intéressant de **normaliser ces bases**.
Elles devront contenir suffisamment d'exemple pour éviter le **sur-apprentissage** du modèle, en particulier si l'on travaille dans un
espace des caractéristiques de grande dimension.
Il peut être utile de **sélectionner ces caractéristiques** pour limiter ce problème.
Nous avons vu quelques moyens de sélectionner manuellement ces caractéristiques.
Enfin, il pourrait être intéressant de **pré-traiter** certaines 
caractéristiques catégorielles pour les encoder en **One Hot Vector**.
    
- Un algorithme d'apprentissage automatique est le plus souvent un **modèle**
qui comporte des **paramètres**.
Le modèle doit être suffisamment complexe pour éviter le
**sous-apprentissage**.
Les paramètres, quant à eux, sont adaptés au problème posé, lors de
l'apprentissage.
Celui ci consiste en une minimisation d'une **fonction de coût**
par un **algorithme d'optimisation**.
Nous avons envisagé des exemples de fonctions de coût pour les trois problèmes
classiques en apprentissage automatique, et expliqué le fonctionnement des
algorithmes d'optimisation, souvent basés sur une **descente de gradient**.
La descente du gradient trouve un **minimum local** de la fonction de coût.

Enfin, la seconde partie de ces pages présentait
**différents algorithmes classiques** pour chacun des trois problèmes.
Je les rappelle ici.
L'objectif est que pour chaque technique, vous soyez capable de décrire son
fonctionnement, même de façon sommaire.

- **Classification** : knn, classification bayésienne, SVM, arbres de décision, réseaux de neurones denses.
- **Régression** : knn, régression linéaire, SVR, arbres de décision, réseaux de neurones denses.
- **Clustering** : CAH, Kmeans, DBSCAN. Dans le cas de certains algorithmes, il faut choisir préalablement le nombre de clusters. Nous avons décrit quelques méthodes pour ce faire (méthode du gap, méthode du coude, indice de silhouette). Insistons encore une fois pour signaler qu'il convient d'inspecter manuellement les clusters pour juger de leur performance. Une évaluation purement numérique est souvent insuffisante.

Avec cet ensemble de connaissance, vous êtes en mesure de répondre à de nombreux problèmes pratiques d'apprentissage automatique. Nous pourrons tester ceci lors des **Travaux Pratiques**. Vous pouvez également aller lire, avant les Travaux Pratiques, les **pages de niveau 2**, qui apportent quelques précisions ainsi que quelques algorithmes ou techniques un peu plus complexes.




