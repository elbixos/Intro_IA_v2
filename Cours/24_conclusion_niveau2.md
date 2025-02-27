<script type="text/javascript" async src="//cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

## Conclusion des pages de niveau 2

Dans ces pages ont été réunies un certain nombre de notions un peu plus
avancées mathématiquement et offrant parfois un changement de vision des
problèmes, dans l'objectif de vous donner une intuition plus grande des
raisons des succès ou des échecs des techniques que vous employez.

- Ainsi, si vous aviez découvert auparavant la notion de malédiction de la
dimensionnalité, nous avons vu qu'il était éventuellement possible de réduire
cette dimensionnalité à l'aide de techniques adaptées (**ACP** ou **auto encodeurs**).
- La présentation des notions de **biais dans les bases** vous a permis de
découvrir un problème qui occupe grandement les chercheurs en intelligence
artificielle  :
éviter que les algorithmes apprennent à reproduire les comportements néfastes
des humains qui ont permis de constituer les bases d'exemples.
- Enfin, nous avons évoqué la possibilité de créer des exemples, via
l'**augmentation de données**, dans le but de diversifier les exemples sur
lesquels les algorithmes apprennent.

La seconde section, dédiée aux algorithmes vous a présenté

- tout d'abord une interprétation possible des sorties d'un classifieur comme
une **probabilité d'appartenance à une classe**.
On retrouve souvent cette démarche dans les réseaux de neurones, notamment. 

La suite de cette section présentait des méta-algorithmes extrêmement élégants.

- Dans le premier cas, on souhaitait utiliser un algorithme de classification binaire  pour une application multi-classes, et nous avons vu qu'on pouvait
utiliser plusieurs classifieurs binaires pour cela, dans une approche
**1 vs 1** ou **1 vs rest**.
- Dans le second cas, il s'agissait d'améliorer les performances en combinant plusieurs classifieurs avant de prendre une décision (le **bagging** utilise
de nombreux classifieurs similaires qui votent, alors que le **boosting** va
définir des classifieurs successifs permettant de corriger partiellement les
décisions prises par les précédents)

Enfin, la dernière section vous a présenté des notions théoriques très
générales sur les algorithmes d'apprentissage automatique.
- Ainsi, la notion de **compromis biais / variance** permet d'éclairer le
problème du **sur-apprentissage** sous un jour nouveau, mais aussi souvent de
mieux percevoir pourquoi un algorithme a de meilleures performances qu'un
autre.
Cela permet aussi de comprendre les intérêts spécifiques du **boosting** et du **bagging**.
- De même, comprendre l'intérêt de **fonctions de coût lisses** a permis de 
nombreux progrès en apprentissage automatique.
- Et pour finir, vous avez découvert une technique assez générale 
d'amélioration des modèles, que l'on croise très souvent en optimisation :
la **régularisation des modèles**.

Tout ceci fournit une base culturelle primordiale, à mon sens, pour qui veut travailler en apprentissage automatique. Le survol des techniques, des façons d'envisager les problèmes, tel qu'ils ont été conçu historiquement, vous permettra, je l'espère, d'oeuvrer à trouver, ou du moins à mieux comprendre, ce qui sera découvert et utilisé à l'avenir.








