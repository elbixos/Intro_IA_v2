<script type="text/javascript" async src="//cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

## Introduction et Objectifs

Si les pages de bases vous ont normalement permis de comprendre dans les grandes lignes les principes de l'apprentissage automatique, dans ces pages, nous allons entrer un peu plus dans le détail technique de certains points. Le niveau de mathématiques utilisé va monter un petit peu, sans être démesuré.

J'ai divisé ces pages en grandes fonctionnalités concernant : **les bases d'exemples** / **les performances** / **les algorithmes**. (Les pages de niveaux 2 suivront le même plan en enrichissant ces points.)

Dans ce chapitre, voici ce que contiendront les différentes parties, si vous voulez sauter directement à un point précis (en première lecture, je recommande de suivre l'ordre proposé) :

- bases d'exemples :
    - Je commencerais par revenir sur la **séparation des bases**,
    - puis présenterais les différents **types de caractéristiques** possibles,
    - avant de voir l'intérêt de la **normalisation des caractéristiques**.
    - Nous parlerons ensuite de la **malédiction de la dimensionnalité**,
    - et de l'éventuelle **sélection manuelle des caractéristiques**.
- performances :
    - Je présenterais les différentes **mesures de performances possibles** pour les algorithmes d'apprentissage automatique
    - je préciserais les notions de **complexité du modèle**, de **sous-apprentissage** et de **sur-apprentissage**
    - Enfin, je décrirais l'**algorithme de descente de gradient** qui sert de base à la plupart des optimisations de nos algorithmes.
- algorithmes :
    - nous verrons les **algorithmes classiques** pour chacune des 3 tâches en apprentissage automatique.


Ceci représente un programme assez conséquent, à l'issue duquel vous devriez avoir une culture tout à fait raisonnable du domaine de l'apprentissage automatique.
